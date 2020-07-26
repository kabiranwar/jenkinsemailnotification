pipeline {

  agent any

  options {
    timestamps()
    withFolderProperties()
  }
  environment {
    TIMESTAMP = new java.text.SimpleDateFormat('yyyyMMdd').format(new Date())
    image = "dummy_image"
    //version = "2.1.0"
    //version = "0.1.${env.BUILD_NUMBER}"
    version = "${params.version}"
    nextversion = "${params.nextversion}"
    branch_name="${env.GIT_BRANCH}".replace("origin/","")
    tag = "${branch_name =="master" ? "${version}" : "${version}-${branch_name}"}"
    imageTag = "/${image}:${tag}"
    GIT_CRED=credentials('github-account-username-password')
  }

  stages {   
    stage('Cleanup') {
      steps {
        deleteDir()
        
      }
    }

    stage('Checkout') {
      steps {
        //checkout scm
        script {
          sh 'rm -Rf test-sourcecode/*'
          repo = checkout([
            $class: 'GitSCM',
            branches: [
              [name: '*/master']
            ],
            doGenerateSubmoduleConfigurations: false,
            userRemoteConfigs: [[
              credentialsId: 'github-account-username-password',
              url: "https://github.com/kabiranwar/test-sourcecode.git"
            ]],
            extensions: [
              [$class: 'CleanBeforeCheckout'], 
              [$class: 'RelativeTargetDirectory', relativeTargetDir:'test-sourcecode']
            ],
            submoduleCfg: [],
          ])
        }
      }
    }
    

  }
  /*post {
    success {
      cleanWs deleteDirs: true
    }
  }*/

  post {
        always {
             emailext attachLog: true, body: """<p>${currentBuild.currentResult}: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p><p>View console output at "<a href="${env.BUILD_URL}"> ${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"</p><p><i>(Build log is attached.)</i></p>""",compressLog: true,replyTo: 'do-not-reply@gmail.com',subject: "Status: ${currentBuild.result?:'SUCCESS'} - Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'",to: 'anwarkabir2011@gmail.com'
            }
    }
    /*post {
    success {
        emailext  subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                to: "anwarkabir2011@gmail.com"
      cleanWs deleteDirs: true
    }
    
    failure {
        emailext  subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                to: "anwarkabir2011@gmail.com"
      cleanWs deleteDirs: true
    }
  }*/
}
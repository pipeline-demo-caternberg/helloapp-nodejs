pipeline {
  agent none
  options { 
    buildDiscarder(logRotator(numToKeepStr: '2'))
   // skipDefaultCheckout true
  }
  stages {
    stage('Web Tests') {
      agent {
        kubernetes {
          label 'nodejs'
          yamlFile 'nodejs-pod.yaml'
        }
      }
      stages {
        stage('Nodejs Setup') {
          steps {
          //  checkout scm
            container('nodejs') {
              sh """
                npm i -S express pug
                node ./hello.js &
              """
            }
          }   
        }
/*        stage('Testcafe') {
          steps {
            container('testcafe') {
              sh '/opt/testcafe/docker/testcafe-docker.sh "chromium --no-sandbox" tests/*.js -r xunit:res.xml'
              //publishChecks name: 'example', title: 'Pipeline Check', summary: 'check through pipeline', text: 'you can publish checks in pipeline script', detailsURL: 'https://github.com/jenkinsci/checks-api-plugin#pipeline-usage'

            }
          }   
        }*/
      }  
      post {
        success {
          stash name: 'app', includes: '*.js, public/**, views/*, Dockerfile'
        }
        always {
          junit 'res.xml'
        }
      } 
    }

  }
}

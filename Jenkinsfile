pipeline {
    agent none
    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
        skipDefaultCheckout true
    }
    stages {
        stages {
            stage('Nodejs Setup') {
                agent {
                    kubernetes {
                        label 'nodejs'
                        yamlFile 'nodejs-pod.yaml'
                    }
                }
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
        }
    }
}

#!groovy​

pipeline {
    agent {
        label 'w64' // this is Windows pipeline
    }
    tools {
        ant "ant-1.9.7"
        jdk "jdk-1.8"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        disableConcurrentBuilds()
    }

    stages {
        stage("Checkout") {
            steps {
                checkout scm
                sh 'git submodule update --init' 
                stash(name:'scripts', includes:'**')
            }
        }

        stage("Build") {
            steps {
                unstash 'scripts'
                timeout(time:2, unit:'MINUTES') {
                    bat 'ant build test'
                    archive 'build/assets/**'
                }
                stash(name:'assets', includes:'build/assets/**')
            }
        }

        stage("Deploy to QA") {
            steps {
                unstash 'assets'
                echo "Deploy"
            }
        }

        stage("Integration Test") {
            steps {
                unstash 'scripts'

                timeout(time:1, unit:'MINUTES') {
                    bat 'ant test -Denv=qa'
                }
            }
            post {
                success {
                    junit 'build/tests/**/TEST-*.xml'
                }
                unstable {
                    junit 'build/tests/**/TEST-*.xml'
                }
            }
        }   

        stage("Manual Testing") {
            steps {
                echo "Manual testing"
            }
        }                 

        stage("Deploy to Prod") {
            steps {
                echo "Deploy"
            }
        }       
    }
}

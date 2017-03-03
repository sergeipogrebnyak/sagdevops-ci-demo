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
            agent {
                label 'master'
            }
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
                    bat 'ant build'
                    archive 'build/assets/**'
                }
                stash(name:'assets', includes:'build/assets/**')
            }
        }

        stage("Integration Test") {
            steps {
                unstash 'scripts'

                timeout(time:10, unit:'MINUTES') {
                    bat 'ant up deploy test'
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

        stage("QA Testing") {
            steps {
                timeout(time:10, unit:'MINUTES') {
                    bat 'ant up deploy test -Denv=qa'
                }
            }
        } 

        stage("Manual Testing and Approval") {
            steps {
                input "Ready to deploy to Production?"
            }
        } 

        stage("Deploy to Prod") {
            steps {
                timeout(time:10, unit:'MINUTES') {
                    bat 'ant up deploy -Denv=prod'
                }
            }
        }       
    }
}

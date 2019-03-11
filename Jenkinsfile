pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'ls -al ./target'
                stash includes: "**/target/*.jar", name: 'appjar'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'alpine:3.7'
                }
            }
            steps {
                sh 'ls -al .'
                unstash name:'appjar'
                sh 'ls -al ./target'
            }
        }
    }
}


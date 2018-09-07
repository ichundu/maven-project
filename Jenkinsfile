pipeline {
    agent any
    stages {

        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging') {
            steps {
                build job: "deploy-to-staging"
            }
        }

        stage ('Deploy to Production') {
            steps {
                tiemout (time: 5, unit: 'DAYS') {
                    input message: "Approve PRODUCTION deployment"
                }

                build job: "deploy-to-produciton"
            }

            post {
                success {
                    echo 'Code successfully deployed to production'
                }

                failure {
                    echo 'Deployment failed!!!'
                }
            }
        }
    }
}

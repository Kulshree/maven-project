#!/usr/bin/env groovy

pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.222.248.168', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.191.244.75', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

 stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "scp -i C:/Users/kulshree.agrawal/Documents/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i C:/Users/kulshree.agrawal/Documents/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
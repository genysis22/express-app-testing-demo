#!groovy
import groovy.json.JsonSlurper
pipeline {
    agent any
    environment {
     def scannerHome = tool 'sonarScanner'
   }

    stages{
        stage('checkout'){
            steps{
                checkout scm
            }
        }
        stage ('build'){
            steps{
                sh 'npm install'
            }
        }
        stage('test'){
            steps{
                sh '''npm test || :
                npm npm run test:e2e || :'''
            }
        }
        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv('sonarQube'){
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.host.url=${SONAR_HOST_URL}  -Dsonar.login=${SONAR_AUTH_TOKEN}  -Dsonar.projectName=express-app -Dsonar.projectVersion=1.0 -Dsonar.projectKey=express-app -Dsonar.sources=$JOB_NAME/masetr/app"
                }

            }
        }
     }
  }

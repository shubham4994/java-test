#!/usr/bin/env groovy
 pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
        
        // function_name = 'java-sample'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn package'
            }
        }

        stage('sonarqube analysis'){
            steps {
                withSonarQubeEnv('Sonar-test'){
                    sh 'mvn sonar:sonar'
                }            }
        }
        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://angular-ui-sj"
            }
        }

        // stage('Deploy') {
        //     steps {
        //         echo 'Build'
        //         sh "aws lambda update-function-code --function-name $function_name --region us-east-1 --s3-bucket bermtecbatch31 --s3-key sample-1.0.3.jar"
        //     }
    }
}

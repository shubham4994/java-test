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
        
        stage('sonarqube analysis') {
            when {
                anyOf {
                    branch 'feature/*'
                    branch 'main'
                }
            }
            steps {
                withSonarQubeEnv('Sonar-test') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Quality gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
        stage('Deployment') {
            parallel {
                stage('Test') {
                    steps {
                        echo 'Deployment to Test'
                    }
                }
            
                stage('UAT Deployment') {
                    steps {
                        echo 'Deployment to UAT'
                    }
                }
            }
        }
        
        stage('Push') {
            when {
                anyOf {
                    branch 'main'
                }
            }
            steps {
                echo 'Push'
                sh "aws s3 cp target/sample-1.0.3.jar s3://angular-ui-sj"
            }
        }
    }
}

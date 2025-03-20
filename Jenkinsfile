pipeline {
    agent any
    environment {
        SONARQUBE_URL = "http://107.20.112.137:9000/"
    }
    stages {
        stage('Checkout Code') {
            steps {
                script {
                    git branch: "dev",
                        url: 'https://github.com/Abimbola-star/NumberGuessGamePractice.git'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube_Token') {
                        sh '''
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=PipelineJob \
                        -Dsonar.projectName='PipelineJob' \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONAR_AUTH_TOKEN}
                        '''
                    }
                }
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                script {
                    deploy adapters: [tomcat7(credentialsId: 'Tomcat_credentials', path: '', url: 'http://54.237.251.55:8080/manager/html')], contextPath: null, war: '**/*.war'
                }
            }
        }
    }
    post {
        success {
    echo ':white_check_mark: Build, Testing, SonarQube Analysis, and Deployment Successful!'
    emailext(
        subject: "Build Success",
        body: "Build was successful",
        to: "aroyewunbimbola@gmail.com"
        )
        failure {
            echo ':x: Build Failed! Check logs for issues.'
        }
    }
}
}

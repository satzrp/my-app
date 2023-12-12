pipeline {
    agent { label 'aws'}

    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Build') {
            steps {
                checkout scm
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=my-app"
                }
            }
        }
    }
}
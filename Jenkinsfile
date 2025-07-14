pipeline {
    agent any

    tools {
        maven 'Maven' // Doit correspondre à ton outil Maven dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('mon_token_sonar') // ID du token dans Jenkins Credentials
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarServer') { // Doit correspondre au nom du serveur Sonar dans Jenkins
                    sh '''
                        mvn sonar:sonar \
                          -Dsonar.projectKey=simple-java-maven-app \
                          -Dsonar.host.url=http://sonarqube:9000 \
                          -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}

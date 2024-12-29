pipeline {
    agent any

    environment {
        SONAR_SCANNER_PATH = 'C:\\Program Files\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    def mvnHome = tool 'Maven3' // Use the Maven tool configured in Jenkins
                    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
                        bat 'mvn clean verify'
                    }
                }
            }
        }

        stage('Sonar Analysis') {
            environment {
                SONAR_TOKEN = credentials('maven_sonar_project_token') // Ensure the correct credential ID
            }
            steps {
                script {
                    def mvnHome = tool 'Maven3' // Use the Maven tool configured in Jenkins
                    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
                        bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=tictacto_java_project_pipeline ^
                        -Dsonar.projectName='tictacto_java_project_pipeline' ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.login=%SONAR_TOKEN%
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/immehdich/todo-app.git'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        /home/devops/.jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner \
                        -Dsonar.projectKey=immehdich_todo-app \
                        -Dsonar.organization=immehdich \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t todo-app .'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker stop todo-app || true'
                sh 'docker rm todo-app || true'
                sh 'docker run -d -p 3000:3000 --name todo-app todo-app'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

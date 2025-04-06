pipeline {
    agent any
    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
                }
            }
        
        stage('Docker Build') {
            steps {
                    sh(script: 'docker compose build')
            }
        }
        stage('Start Application') {
            steps {
                sh(script: 'docker compose up -d')
            }
        } /*
        stage('Run Tests') {
            steps {
                sh(script: 'pytest ./tests/test_sample.py')
            }
            post {
                success {
                    echo 'Tests passed!'
                }
                failure {
                    echo 'Tests failed!'
                    sh(script: 'docker compose down')
                }
            }
        }
        */
        stage('Push to Docker Hub') {
            steps {
                echo "Running in $WORKSPACE"
                dir("$WORKSPACE/azure-vote") {
                    echo "Pushing to Docker Hub"
                    script {
                        docker.withRegistry('', 'dockerpat') {
                            def image = docker.build("mikevelasco16/azure-vote:${env.BUILD_ID}")
                            image.push()
                            echo "Pushed to Docker Hub"
                        }
                    }
                }
            }
        }
    }
}
post {
        always {
            echo 'Cleaning up...'
            sh(script: 'docker compose down')
        }
    }
// ThisJenkinsfile is for a simple CI/CD pipeline that builds a Docker image, starts the application, and runs tests.
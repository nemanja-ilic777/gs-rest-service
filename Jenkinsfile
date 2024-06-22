pipeline {
    agent any

    stages {
        stage('Build and Package') {
            steps {
                // Checkout the code from Git
                git 'https://github.com/nemanja-ilic777/gs-rest-service.git'
                
                // Maven build
                sh 'mvn clean package'
                
                // Archive artifacts for future use (optional)
                archiveArtifacts artifacts: 'complete/target/*.jar', allowEmptyArchive: true
            }
        }
        
        stage('Transfer JAR to Project Server') {
            steps {
                // Copy JAR file from Jenkins to project server
                script {
                    sh 'scp /var/lib/jenkins/workspace/gs-rest-service-main-ci/complete/target/rest-service-complete-0.0.1-SNAPSHOT.jar nimaya@server-project:/home/nimaya/gs-rest-service/complete/target/'
                }
            }
        }
        
        stage('Deploy to Docker') {
            steps {
                // Copy JAR file from project server to Docker container
                script {
                    sh 'sudo docker cp /home/nimaya/gs-rest-service/complete/target/rest-service-complete-0.0.1-SNAPSHOT.jar cranky_elbakyan:/'
                    sh 'sudo docker restart cranky_elbakyan'
                }
            }
        }
    }
    
    // Optionally, post actions after all stages are completed
    post {
        success {
            // Example: Notify success to Slack
            slackSend color: 'good', message: "Pipeline successfully deployed to Docker!"
        }
        failure {
            // Example: Notify failure to Slack
            slackSend color: 'danger', message: "Pipeline deployment to Docker failed!"
        }
    }
}


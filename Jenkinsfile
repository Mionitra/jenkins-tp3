pipeline {
    agent any
    environment {
        IMAGE_TAG = "${env.BUILD_ID}"
    }
    stages {
        stage('Pull, Scan and Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh 'ansible-playbook push_images.yml'
                }
            }
        }
    }
    post {
        always {
            // Archive the CSV so it's visible in the Jenkins build artifacts UI
            archiveArtifacts artifacts: 'output_*.csv', allowEmptyArchive: true
        }
        failure { echo 'Pipeline failed — check outputs.csv for vulnerability details.' }
        success { cleanWs() }
    }
}

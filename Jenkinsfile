pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "My first pipeline"'
                sh '''
                    echo "By the way, I can do more stuff in here"
                    ls -lah
                '''
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing!"'
            }
        }
        stage('Staging') {
            when{
                branch "staging"
            }
            steps {
                sh 'echo "Staging!"'
            }
        }
    }
    post {
        always {
            echo 'I will always get executed'
        }
        success {
            echo 'I will only get executed if this success'
        }
        failure {
            echo 'I will only get executed if this fails'
        }
        unstable {
            echo 'I will only get executed if this is unstable'
        }
    }
}
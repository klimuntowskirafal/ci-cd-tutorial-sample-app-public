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
        stage("PR's"){
            when{
                branch "PR-*"
            }
            steps{
                echo 'this only runs for PRs'
            }
        }
        stage('Staging') {
            when{
                branch "staging"
            }
            steps {
                sh 'echo "Staging!"'
                sh 'cat seed.py'
            }
        }
        stage('Production') {
            when{
                branch "main"
            }
            steps {
                sh 'echo "Production on Main!"'
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
pipeline {
    agent any
    environment {
        CREDS_FOR_AWS = credentials("ci-cd-tutorial-sample-app-public-aws-creds")
//         STACK_INFO = "aws cloudformation describe-stacks --region eu-central-1 --stack-name cicd-example-stack-name-test"
    }
    options { disableConcurrentBuilds() }
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
//                 echo "${env.STACK_INFO}"
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
                /*
                sh 'aws cloudformation create-stack \
                    --stack-name cicd-example-stack-name-staging \
                    --template-body file://./aws-cf-ecs-template.yaml \
                    --capabilities CAPABILITY_NAMED_IAM \
                    --region eu-central-1 \
                    --parameters ParameterKey=SubnetID,ParameterValue=subnet-d76dc19b ParameterKey=ImageName,ParameterValue=025628008566.dkr.ecr.eu-central-1.amazonaws.com/flask-app-image:latest'
                */
            }
        }
        stage('Production') {
            when{
                branch "main"
            }
            steps {
                sh 'echo "Production on Main!"'
                script {
                    try {
                        sh 'aws cloudformation create-stack \
                        --stack-name cicd-example-stack-name-production \
                        --template-body file://./aws-cf-ecs-template.yaml \
                        --capabilities CAPABILITY_NAMED_IAM \
                        --region eu-central-1 \
                        --parameters ParameterKey=SubnetID,ParameterValue=subnet-d76dc19b ParameterKey=ImageName,ParameterValue=025628008566.dkr.ecr.eu-central-1.amazonaws.com/flask-app-image:latest'
                    }
                    catch (AlreadyExistsException) {
                        echo 'Stack already exist'
                        echo 'Trying to update'
                        try {
                            sh 'aws cloudformation update-stack \
                            --stack-name cicd-example-stack-name-production \
                            --template-body file://./aws-cf-ecs-template.yaml \
                            --capabilities CAPABILITY_NAMED_IAM \
                            --region eu-central-1 \
                            --parameters ParameterKey=ImageName,UsePreviousValue=true ParameterKey=SubnetID,ParameterValue=subnet-d76dc19b'
                        }
                        catch (ValidationError) {
                            echo 'Nothing to be updated on the stack'
                        }
                    }
                }
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

pipeline{
    agent any
    stages{
        stage('Create Infrastructure for the App') {
            steps {
                echo 'Creating Infrastructure for the App on AWS Cloud by using CF Template'
                sh "aws cloudformation create-stack --stack-name tyler-app --template-body file://cfn-template-to-create-k8s-cluster.yml --region 'us-east-1'" 
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
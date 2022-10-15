pipeline{
    agent any
    stages{
        stage('Create Infrastructure for the App') {
            steps {
                echo 'Creating Infrastructure for the App on AWS Cloud by using CF Template'
                sh "aws cloudformation create-stack --stack-name tyler-app --template-body file://cfn-template-to-create-k8s-cluster.yml --region 'us-east-1' --capabilities CAPABILITY_NAMED_IAM" 
            }
        }


        stage('wait the instance') {
            steps {
                script {
                    echo 'Waiting for the instance'
                    id = sh(script: 'aws ec2 describe-instances --filters Name=tag-value,Values=Kube-Master-tyler-app Name=instance-state-name,Values=running --query Reservations[*].Instances[*].[InstanceId] --output text',  returnStdout:true).trim()
                    sh 'aws ec2 wait instance-status-ok --instance-ids $id'
                }
            }
        }


        stage('Deploy App on Petclinic Kubernetes Cluster'){
            steps {
                echo 'Deploying App on K8s Cluster'
                sh "helm repo add to-do-app https://github.com/TylerCounter/mygithubrepo.git"
                sh "helm repo update"
                sh " helm upgrade --install serkan-task-app-release serkan_chart "
            }
        }


    }


}
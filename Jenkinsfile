pipeline {
    agent any
    stages {
        stage('git pull') { 
            steps {
                git 'https://github.com/deepak-umre/studentapp.ui.git'
            }
        }
        stage('mvn build') { 
            agent {
                label 'mvn'
            }
            steps {
                sh 'mvn clean package'
            }
        }
         stage('docker'){
            agent {
                label 'mvn'
            }
            steps {
                sh '''
                    cd /home/ubuntu/workspace/k8/
                    sudo docker login
                    sudo docker build -t shivampandhare/jentom:latest -f Dockerfile .
                    sudo docker push shivampandhare/jentom:latest
                '''
            }
        }
        stage('deploy') {
            agent {
                label 'kube'
            }
            steps {
                sh '''
                cd /home/ec2-user/
                git init
                git pull https://github.com/Shivampandhare/jenkins-demo-1.git master
                sudo kubectl apply -f deploy.yaml
                '''
            }
        }
    }
}

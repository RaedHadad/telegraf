pipeline {
    agent any

    parameters {
        string(name: 'EC2_HOST', defaultValue: '', description: 'Enter the EC2 public IP or DNS name')
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/RaedHadad/telegraf.git'
            }
        }

        stage('Install Telegraf on EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                        echo "Uploading files to EC2 instance..."
                        scp -i $SSH_KEY -o StrictHostKeyChecking=no install_telegraf.sh telegraf.conf $SSH_USER@$EC2_HOST:/tmp/

                        echo "Running installation script on EC2..."
                        ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@$EC2_HOST 'sudo bash /tmp/install_telegraf.sh'
                    '''
                }
            }
        }
    }
}

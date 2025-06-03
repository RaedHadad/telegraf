pipeline {
    agent any

    environment {
        EC2_HOST = "your-ec2-ip" // Replace with actual public IP or DNS
    }

    stages {
        stage('Checkout Repo') {
            steps {
                git 'https://github.com/RaedHadad/telegraf.git'
            }
        }

        stage('Install Telegraf on EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ubuntu', keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
                    sh '''
                    # Upload files
                    scp -i $SSH_KEY -o StrictHostKeyChecking=no install_telegraf.sh telegraf.conf $SSH_USER@$EC2_HOST:/tmp/

                    # SSH and install
                    ssh -i $SSH_KEY -o StrictHostKeyChecking=no $SSH_USER@$EC2_HOST 'sudo bash /tmp/install_telegraf.sh'
                    '''
                }
            }
        }
    }
}

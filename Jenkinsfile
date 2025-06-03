pipeline {
    agent any

    parameters {
        string(name: 'EC2_HOST', defaultValue: '', description: 'Enter the EC2 public IP or DNS name')
    }

    stages {
        stage('Test SSH Connection to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',      // This must match your Jenkins SSH credential ID
                    keyFileVariable: 'SSH_KEY',
                    usernameVariable: 'SSH_USER'
                )]) {
                    sh '''
                        echo "🔐 SSH_KEY: $SSH_KEY"
                        echo "👤 SSH_USER: $SSH_USER"
                        echo "🌐 EC2_HOST: $EC2_HOST"
                        
                        echo "📡 Attempting SSH connection..."
                        ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no "$SSH_USER@$EC2_HOST" "echo ✅ SSH connection from Jenkins succeeded"
                    '''
                }
            }
        }
    }
}

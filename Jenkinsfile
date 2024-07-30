pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy') {
            steps {
                withCredentials([sshUserPrivateKey(
                    credentialsId: 'counter-app-ec2user',
                    keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        
                        scp -i $SSH_KEY -r build/* ec2-user@13.232.83.182:/var/www/html
                        ssh -i $SSH_KEY ec2-user@13.232.83.182 'sudo systemctl restart httpd'
                    '''
                }
                // Commands to deploy your build, e.g., copying files to a server
            }
        }
    }
}
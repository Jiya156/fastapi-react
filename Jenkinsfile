pipeline {

    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Pulling latest code...'
                checkout scm
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo 'Running deployment...'
                sh '''
                    ansible-playbook \
                    -i /etc/ansible/fastapi-react/inventory.ini \
                    /etc/ansible/fastapi-react/deploy.yml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking services...'
                sh '''
                    ssh -o StrictHostKeyChecking=no azureuser@APP_VM_IP \
                    "sudo systemctl is-active fastapi && sudo systemctl is-active nginx"
                '''
            }
        }

    }

    post {

        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }

    }
}
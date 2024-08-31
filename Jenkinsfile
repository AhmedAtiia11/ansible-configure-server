pipeline {
    agent any
    environment {
        // Define the Ansible playbook path and inventory
        ANSIBLE_PLAYBOOK = 'play.yml'
        ANSIBLE_INVENTORY = 'inventory'
    }
    stages {
        stage('Setup Ansible') {
            steps {
                script {
                    // Ensure Ansible is installed
                    sh '''
                        if ! command -v ansible > /dev/null; then
                            echo "Ansible not found. Installing..."
                            sudo apt update
                            sudo apt install -y ansible
                        fi
                    '''
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Run the Ansible playbook
                    sh """
                        ansible-playbook -i ${ANSIBLE_INVENTORY} ${ANSIBLE_PLAYBOOK}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

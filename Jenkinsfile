pipeline {
    agent any
    environment {
        // Define the Ansible playbook path , inventory , Container name and Host port
        ANSIBLE_PLAYBOOK = 'play.yaml'
        ANSIBLE_INVENTORY = 'inventory'
        CONTAINER_NAME = 'tomcat_container'
        HOST_PORT = '8082'
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
        stage('Check and Remove Existing Container') {
            steps {
                script {
                    // Check if the container exists (running or stopped)
                    def containerExists = sh(script: "docker ps -a --filter 'name=${CONTAINER_NAME}' --format '{{.Names}}'", returnStdout: true).trim()

                    if (containerExists) {
                        // Stop and remove the existing container
                        sh "docker stop ${CONTAINER_NAME} || true"
                        sh "docker rm ${CONTAINER_NAME}"
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run a new container
                    sh """
                    docker run -d --name ${CONTAINER_NAME} \
                    -p ${HOST_PORT}:8080 \
                    -v ./sample.war:/usr/local/tomcat/webapps/sample.war \
                    tomcat
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

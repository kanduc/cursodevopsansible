pipeline {
    agent any

    stages {
        stage('Configure with Ansible') {
            agent {
                docker {
                    image 'jenkins-ansible:1.0.0'
                    args '--entrypoint="" -u root -v ${WORKSPACE}:/src'
                }
            }                    
            steps {
                script {
                    sh "ansible-playbook main.yml -i hosts -v"
                }
            }
        }

        stage ('post-despliegue') {
            steps {
                cleanWs()
            }
        }
    }
}
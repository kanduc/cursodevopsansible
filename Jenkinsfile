pipeline {
    agent any

    stages {
        stage('Test Ansible') {
            agent {
                docker {
                    image 'jenkins-ansible:1.0.0'
                    args '--entrypoint="" -u root -v ${WORKSPACE}:/src'
                }
            }                    
            steps {
                script {
                    sh "ansible --version"
                    sh "az version"
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
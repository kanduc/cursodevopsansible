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
                    withCredentials([azureServicePrincipal('sp-iac-azure')]){
                        echo "Iniciando sesion Azure"

                        sh "az account clear"
                        sh "az login --service-principal --username ${AZURE_CLIENT_ID} --password ${AZURE_CLIENT_SECRET} --tenant ${AZURE_TENANT_ID}"
                        sh "az account set --subscription ${AZURE_SUBSCRIPTION_ID}"

                        sh "ansible-playbook main.yml -i hosts -e TIPO_TAREA="+env.TYPE_TASK+" -e subscription_id=${AZURE_SUBSCRIPTION_ID} -e client_id=${AZURE_CLIENT_ID} -e secret_id=${AZURE_CLIENT_SECRET} -e tenant_id=${AZURE_TENANT_ID} -v"
                    }
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
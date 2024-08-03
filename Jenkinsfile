pipeline {
    agent any

    stages {
        stage('Aprovisionando Ansible') {
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

                    withCredentials([azureServicePrincipal('sp-iac-azure')]){
                        echo "Iniciando sesion en Azure"

                        sh "az account clear"
                        sh "az login --service-principal --username ${AZURE_CLIENT_ID} --password ${AZURE_CLIENT_SECRET} --tenant ${AZURE_TENANT_ID}"
                        sh "az account set --subscription ${AZURE_SUBSCRIPTION_ID}"

                        echo "Creando VM en Azure"

                        sh "ansible-playbook site.yml -e subscription_id=${AZURE_SUBSCRIPTION_ID} -e client_id=${AZURE_CLIENT_ID} -e secret_id=${AZURE_CLIENT_SECRET} -e tenant_id=${AZURE_TENANT_ID} -v"

                        echo "Validando VM en Azure"

                        sh ''' az vm list -d -o table --query "[?name=='myVM']" '''
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
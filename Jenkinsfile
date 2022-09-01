pipeline {
    agent any

    stages {
        stage('Cloning git') {
            steps {
                script { 
                    try { 
                  git branch: 'main', credentialsId: 'faizank789', url: 'git@github.com:faizank789/minio_pipeline.git'
            }
             catch (Exception errorlogs) {
                    println(errorlogs)
                    echo " Git Cloning issue ! Please check"
                }
            }
            }
        }

     stage('Checking helm Package')  {
        steps {
            script {
                try {
                    sh '''
                    #!/bin/bash
                    helm_binary=`ls /usr/local/bin/helm`
                    if ! [[ $helm_binary ]] ; then
                       curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 && \
                       chmod 700 get_helm.sh && \
                       ./get_helm.sh
                    fi
                    '''
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                    echo " something wrong for helm package please check !"
                }
            }
        }
     }

        stage('Installing Minio with Helm') {
            steps {
            script {   
                try {
                    sh '''
                    #!/bin/bash
                    helm_repo="https://charts.bitnami.com/bitnami"
                    values_path="values.yaml"
                    kubectl get ns minio || kubectl create ns minio
                    helm repo add minio $helm_repo && \
                   helm install minio minio/minio -f $values_path --namespace minio --version 11.9.1 --wait
                  '''
                }
                catch (Exception errorlogs) {
                    println(errorlogs)
                    echo "Registry login issue Please check !"
                }
            }
            }
        }
     }
    }



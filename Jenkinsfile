pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID=''
        AZURE_TENANT_ID=''
        CONTAINER_REGISTRY=''
        RESOURCE_GROUP=''
        REPO="myapp"
        TAG="${currentBuild.number}"
    }
    stages {
        stage('Source'){
            steps{
                git branch: 'main', url: 'https://github.com/krishnadasns/my-app.git'
            }
        }
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'MyApp', passwordVariable: 'AZURE_CLIENT_SECRET', usernameVariable: 'AZURE_CLIENT_ID')]) {
                            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
                            sh 'az account set -s $AZURE_SUBSCRIPTION_ID'
                            sh 'az acr login --name $CONTAINER_REGISTRY --resource-group $RESOURCE_GROUP'
                            sh 'az acr build --registry $CONTAINER_REGISTRY --image $REPO:$TAG .'
                        }
            }
        }
    }
}

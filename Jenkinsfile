pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID='d7d91dda-4b1c-4006-9ed0-67f5788026f3'
        AZURE_TENANT_ID='a86bc255-9bb7-4ee8-b30a-51fba84872aa'
        CONTAINER_REGISTRY='adfolksjenkinstest'
        RESOURCE_GROUP='rg-krishnadas-devops'
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

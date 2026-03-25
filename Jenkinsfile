pipeline {
    agent any
    environment {
        AZURE_SUBSCRIPTION_ID = '5113f0ab-8244-42ec-a2ae-f90527b775e5'
        AZURE_TENANT_ID       = '38a9536a-cdd6-44a4-81d4-b412a633abd6'
        RESOURCE_GROUP        = 'jenkins-get-started-rg'
        WEBAPP_NAME           = 'jenkins-node-app-fyou'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Azure Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'AzureServicePrincipal',
                    usernameVariable: 'AZURE_CLIENT_ID',
                    passwordVariable: 'AZURE_CLIENT_SECRET'
                )]) {
                    sh '''
                        az login --service-principal \
                        --username $AZURE_CLIENT_ID \
                        --password $AZURE_CLIENT_SECRET \
                        --tenant $AZURE_TENANT_ID
                    '''
                }
            }
        }
        stage('Deploy to Azure') {
            steps {
                sh '''
                    zip -r app.zip .
                    az webapp deploy \
                    --resource-group $RESOURCE_GROUP \
                    --name $WEBAPP_NAME \
                    --src-path app.zip \
                    --type zip
                '''
            }
        }
    }
}

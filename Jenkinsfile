#!/usr/bin/env groovy

// options { buildDiscarder(logRotator(numToKeepStr: '5')) }


node {
    stage('Init Stage'){
        env.APP_NAME = "bs-flex-api"
        env.ACR_NAME = "liasomniadevakscicdmvpacr"
        env.SUBSCRIPTION_ID = "ad08cb79-9b11-4db3-91c2-91f2ccec68eb"
	env.BRANCH_NAME = env.BRANCH_NAME.replaceAll('/','_')
    }

    stage('GitCheckout'){
        checkout scm
    }

    stage('AzureBuild'){
    withCredentials([azureServicePrincipal('test-rig-demo-jenkins')]) {
    configFileProvider([configFile(fileId: '76f94832-505f-483c-a37c-e91f62fb41bb', targetLocation: './NuGet.Config')]) {
        sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
        sh 'az acr build --file ./src/Lias.FlexApi/Dockerfile --subscription  ${SUBSCRIPTION_ID}   --registry ${ACR_NAME} --image ${APP_NAME}:${BRANCH_NAME} .'
    }}}


// stage('InitPopulator'){
//     withCredentials([azureServicePrincipal('test-rig-demo-jenkins')]) {
//         sh '''
//             set -e
//             az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
//             az aks get-credentials --resource-group lias-omnia-dev-aks-cicd-mvp-rg --subscription ad08cb79-9b11-4db3-91c2-91f2ccec68eb   --name lias-omnia-dev-aks-cicd-mvp-aks
//             rm -rf ~/DROPDOWNS/
//         '''
//     }}
 
//     stage('PopulateNamespaces'){
//             sh '''
//                 set -e
//                 for NAMESPACE in $(kubectl get namespaces -o json | jq -r '.items[].metadata | select(.name | startswith("self-")) | .name ')
//                 do
//                     echo NAMESPACE is $NAMESPACE
//                     mkdir -p ~/DROPDOWNS/namespaces/${NAMESPACE}
//                 done
//             '''
//     }
 
//     stage('PopulateRepos'){
//         sh '''
//             set -e
//             for REPOSITORY in $(az acr repository list --subscription ad08cb79-9b11-4db3-91c2-91f2ccec68eb --name liasomniadevakscicdmvpacr | jq -r '.[]')
//             do
//                 echo REPO is $REPOSITORY
//                 for TAG in $(az acr repository show-tags --subscription ad08cb79-9b11-4db3-91c2-91f2ccec68eb --name liasomniadevakscicdmvpacr --repository $REPOSITORY | jq -r .[])
//                 do
//                     mkdir -p ~/DROPDOWNS/apps/$REPOSITORY/$TAG
//                 done
//             done
//         '''
//     }

 


 
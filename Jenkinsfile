node {
   stage('Checkout Code') {
      checkout scm
   }
   stage('Push Docker Images to Nexus Registry') {
      echo "Logging into the docker registry\n"
      sh "docker login -u ${params.docker_username} -p ${params.docker_password} 52.175.54.34:8123"
      echo "Building the docker image\n"
      sh "docker build -t python-docs-hello-world ."
      echo "Tagging the image\n"
      sh "docker tag python-docs-hello-world:latest 52.175.54.34:8123/python-docs-hello-world:latest"
      sh "docker push 52.175.54.34:8123/python-docs-hello-world:latest"
    }
   stage('Deploy Docker Image to Azure Web App') {
      def resourceGroup = 'hsbc-finance-forecasting'
      def webAppName = 'ff-webapp'
      def nexusURL = '52.175.54.34:8123'
      // login Azure
      withCredentials([azureServicePrincipal('azure_service_principal')]) {
         sh '''
         az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
         az account set -s $AZURE_SUBSCRIPTION_ID
         '''
      }
      // deploy container

      sh '''
      az webapp config container set --name $webAppName --resource-group $resourceGroup --docker-custom-image-name $nexusURL/python-docs-hello-world:latest --docker-registry-server-url http://swsundar-nexusdocker.eastasia.cloudapp.azure.com:8123/
      '''

      // log out
      sh 'az logout'
   }
}
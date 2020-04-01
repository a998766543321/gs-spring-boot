node {
   stage('init') {
      checkout scm
   }
   stage('Test API') {
      def response = serviceNow_createChange serviceNowConfiguration: [instance: 'exampledev', producerId: 'ls98y3khifs8oih3kjshihksjd'], credentialsId: 'jenkins-vault', vaultConfiguration: [url: 'https://vault.example.com:8200', path: 'secret/for/service_now/']
      echo 'API testing.....'
   }
   stage('build') {
      sh '''
         mvn clean package
         cd target
         cp ../src/main/resources/web.config web.config
         cp spring-boot-0.0.1-SNAPSHOT.jar app.jar 
         zip todo.zip app.jar web.config
      '''
   }
   stage('deploy') {
      azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
      resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/todo.zip"
   }
}
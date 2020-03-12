node {
   stage('init') {
      checkout scm
   }
   stage('build') {
      sh '''
         mvn clean package
         cd target
         cp spring-boot-0.0.1-SNAPSHOT.jar app.jar 
      '''
   }
   stage('deploy') {
      azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
      resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/app.jar"
   }
}
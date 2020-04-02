import groovy.json.JsonSlurper

node {
   stage('init') {
      checkout scm
   }
   stage('Create a change request to SN') {
      
      def response = serviceNow_createChange serviceNowConfiguration: [instance: 'jardineonesolutionhkltddemo4', producerId: '426c66e71bb34cd0b93843f7cc4bcb78'], credentialsId: 'ServiceNowDemo4'
      def jsonSlurper = new JsonSlurper()
      /*
      // Outputs the headers information 
      def responseHeaders = response.headers
      responseHeaders.each { 
         entry -> echo "Key: $entry.key"
         entry.value.eachWithIndex { valEntry, index ->
            echo "Value $index: $valEntry"
         }
      }
      */
      echo response.content
      //def createResponse = jsonSlurper.parseText(response.content)
      echo 'Create a change request to SN complete'
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
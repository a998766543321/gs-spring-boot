import groovy.json.JsonSlurper

node {
   stage('init') {
      checkout scm
   }
   stage('Test API') {
      // Produce a new change?
      def response = serviceNow_createChange serviceNowConfiguration: [instance: 'jardineonesolutionhkltddemo4', producerId: '3Dcb2a927f935002003b7a7a75e57ffb4c'], credentialsId: 'ServiceNowDemo4'
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
      echo 'API testing ends'
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
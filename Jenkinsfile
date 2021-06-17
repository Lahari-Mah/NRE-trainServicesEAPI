pipeline
{
  agent any
  
  environment {
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    WORKER = "Micro"
    APIKEYID = "2ebb6379710f4af39546584157fd896e"
    APIKEYSECRET = "7388e24F6C21434c8c269c1E0a81D110"
  }
  
  stages{
    stage('Build Application'){
      steps{
    		sh 'mvn clean install -DskipTests'
    	}
    }
    
    stage('Deploy Application to MuleSoft CloudHub'){
     environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'NRE-trainServicesEAPI'
      }
     steps{
             sh 'mvn package deploy -DmuleDeploy -DskipTests -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CREDS_PSW -Dcloudhub.app=$APP_NAME -Dcloudhub.environment=$ENVIRONMENT -Dcloudhub.worker=$WORKER -Dapikey.id=$APIKEYID -Dapikey.secret=$APIKEYSECRET'
    	}
    }
    
    stage('Perform Regression Testing'){
      steps{
    		sh 'newman run /Users/rm/Desktop/NjcLabs/newman/getDBSoapDetails.postman_collection.json'
    	 }
    }
  }
}
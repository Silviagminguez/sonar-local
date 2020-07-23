// Pipeline para proyectos Android
//
// V.1.0.0
//

def server_up = false

pipeline {
agent any
   // agent { label "sdk5" }
    stages {
        stage('Build') {
            steps {
		bat "./gradlew build"
            }
        }
	stage('Compile') {
	    steps {
            archiveArtifacts artifacts: '**/*.apk', fingerprint: true, onlyIfSuccessful: true            
	    }
	}
	 stage ('sign apk') { 
	    steps{
	    signAndroidApks ( 
	    keyStoreId: "AndroidSign", 
	    keyAlias: "my-alias", 
	    apksToSign: "**/*-unsigned.apk" 
	    ) 
	    }   
	 }
	 stage('Sonarqube') {
    		environment {
        	scannerHome = tool 'SonarQubeScanner'
    		}
   	    steps {
       		 withSonarQubeEnv('SonarQube') {
           	 bat "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
       	    	}
           }
   	 }

    }
}  

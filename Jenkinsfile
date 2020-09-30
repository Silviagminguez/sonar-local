// Pipeline para proyectos Android
//
// V.1.0.0
//

def server_up = false

pipeline {
	environment{
		URL_PROPERTIES = "https://github.com/Silviagminguez/sonar-properties.git/sonar-scanner-wefferent.properties"
		scannerHome = tool 'SonarQubeScanner'
			
	}
	
	
	agent any
	//    agent { label "sdk5" }
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
		/* stage('Sonarqube') {
			environment {
			scannerHome = tool 'SonarQubeScanner'
			}
		    steps {
			 withSonarQubeEnv('SonarQube') {
			 bat "${scannerHome}/bin/sonar-scanner -X -Dproject.settings=sonar-scanner.properties "
			}
		   }
		 }*/
			
	stage('Checkout Project Sonar Properties') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [], 
                gitTool: 'default', 
                submoduleCfg: [], 
                            userRemoteConfigs: [[
                            credentialsId: 'GithubCredentials',
                            url: "$URL_PROPERTIES"
                        ]]
                ])
            }
              
        }

		    stage('Sonarqube') {
			
		    steps {
			 withSonarQubeEnv('SonarQube') {
				 bat "${scannerHome}/bin/sonar-scanner -X -Dproject.settings='https://github.com/Silviagminguez/sonar-properties.git/sonar-scanner-wefferent.properties'"
			}
		   }
		 }

	    }
	}  

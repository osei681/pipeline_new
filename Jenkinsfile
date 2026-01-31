// Powered by Infostretch 

timestamps {

node ('brendaNode') { 

	stage ('BrendaJob - Checkout') {
 	 checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'gitHubCred', url: 'https://github.com/ayafor-code/javaWebApp.git']]]) 
	}
}
}
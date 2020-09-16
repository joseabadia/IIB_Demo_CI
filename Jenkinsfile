pipeline {
    agent any
	environment
	{
	ant_build = "build.xml"
	}
    stages {
	    
        stage('Checkout') { 
            
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/joseabadia/IIB_Demo_CI.git']]])
            }
        }
        
		stage('Build') {
		steps
		 {
			withAnt(installation: 'apache-ant-1.10.8', jdk: 'jdk8'){
			// some block
			bat "ant -f ${ant_build}"
			}
		  }
		}
		
		stage('Upload') {
		steps {
		script {
			def server = Artifactory.server 'JfrogArtifactory'
			def uploadSpec = """{
			"files": [{
			"pattern": ".\\GeneratedBarFiles\\",
			"target": "jenkins/"
			}]
			}"""
 
			server.upload(uploadSpec)
			
			}
		}
		}
		
		stage('Deploy') {
		steps
		 {
			withAnt(installation: 'apache-ant-1.10.8', jdk: 'jdk8'){
			// some block
			bat "ant -f ${ant_build} deployBAR"
			}
		  }
		}
      
    }
	
	
}

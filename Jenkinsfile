pipeline {
	options {
		buildDiscarder(logRotator(numToKeepStr:'300'))
	}
	parameters {
		choice(
			choices: 'package\ninstall\ndeply',
            description: 'maven goal',
            name: 'GOAL')
		
		choice(
			choices: 'dev_hbo\nhbo/qa\ndeply\nhbo/loadtest\nhbo/prod\nrefs/tags/hbo_release',
            description: 'git branch name',
            name: 'BRANCH')
	}
	environment {
		JOB_NAME = ''
		BUILD_URL = ''
		BUILD_NUMBER = ''
	}
	
	tools {
    
    maven "maven3.2.2"
    jdk "jdk8"
  }
  
  stages {
		
		stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
				
				hipchatSend (color: 'YELLOW', notify: true,
					message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
          )
            }
        }
		
        stage('Build') {
            steps {
                
                sh '''
                    rm -rf ~/.m2/repository/net/malbam/services/registration
                    
                '''
            }
			
			steps {
				sh '''
					mvn clean 
					${params.GOAL} 
					--global-settings=settings.xml 
					-s setting.xml
					-Dmaven.test.skip=true
					-U
					-pl
					registration-common,business
					
					''' 
            }
            
			
        }
	}
  
  post {
    always {
      deleteDir()
    }
	
	success {
      hipchatSend (color: 'GREEN', notify: true,
          message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        )
	}

    failure {
      hipchatSend (color: 'RED', notify: true,
          message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        )
	}
    
  }

}

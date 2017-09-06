pipeline {
    agent any
    
    environment {
            
        BRANCH_NAME = ''
    
    }
    options {
       buildDiscarder(
           logRotator(
               daysToKeepStr: '90'
           )
       )
    }
    stages {
        stage('checkout') {
            steps {
                deleteDir()
            }
        }
        stage('Build') {
            steps {
                if (env.BRANCH_NAME == 'master') {
                    
                    hipchatSend (color: 'YELLOW', notify: true,
                    message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
                   
                    def job = build job: 'userservice-registration', 
                    parameters: 
                    [
                     [
                        $class: 'StringParameterValue', 
                        name: 'GOAL', value: 'install'
                        name: 'BRANCH', value: 'dev_hbo'
                    ]
                   ]
                }
                
            }
        }
            
        steps {
                
                sh '''export PATH=$PATH:/opt/activator-1.3.0
                rm -rf ~/.ivy2/cache/net.malbam.services.registration.mysql
                rm -rf ~/.m2/repository/net/malbam/shared/appcache
                rm -rf ${WORKSPACE}/target
                activator clean
                activator stage '''
        }
        
        steps{
                
                def job = build job: 'Bamzon-Generic-RPM-Builder', 
                        parameters: 
                         [
                             [
                                $class: 'StringParameterValue', 
                                name: 'buildRoot', value: '${WORKSPACE}/target/universal/stage',
                                name: 'rpmUser', value: 'root'
                                name: 'prefixPath', value: '/usr/share/userservices'
                                name: 'rpmDescription', value: 'User service Dev Build',
                                name: 'buildTarget', value: '',
                                name: 'bucket_path', value: 'dev/skynet'
                             ]
                         ]
            }
        }
        
    stage('') {
                steps {
               
            }
        }
 }
post {
    success {
      hipchatSend (color: 'GREEN', notify: true,
          message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        )
    }

    failure {
         hipchatSend (color: 'RED', notify: true,
          message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
        )
        }
    }
}

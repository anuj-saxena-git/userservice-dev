#!groovy

import hudson.model.*
import hudson.EnvVars

node {
  
  // Wipe the workspace so we are building completely clean
  deleteDir()
  
  stage 'Checkout'
  
  checkout scm
  
  properties(
    [
      buildDiscarder(
        logRotator(
          artifactDaysToKeepStr: '', 
          artifactNumToKeepStr: '', 
          daysToKeepStr: '90', 
          numToKeepStr: ''
        )
      ),
      pipelineTriggers(
        [
        ]
      )
    ]
  )

  properties(
    [
      [
        $class: 'BuildDiscarderProperty', 
        strategy: [
          daysToKeepStr: '90', 
        ]
      ]
    ]
  );
  
  
##  stage 'Source Code Management' {
  
##    git changelog: false, credentialsId: 'Anuj GIT', poll: false, url: 'https://github.com/anuj-saxena-git/userservice-dev.git'
  
## }

  stage 'Trigger/Builds on other projects'
  
  if (env.BRANCH_NAME == 'master') {
    build '../other-repo/master'
}
  
  stage 'Post-build Actions'

  

  
}

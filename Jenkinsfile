node {
  
  # Check-box :  Discard old builds 
  
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
  
  stage 'Source Code Management'

  stage 'Trigger/Builds on other projects'
  
  stage 'Post-build Actions'

  

  
}

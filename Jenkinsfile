pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         stage('Security Scan') {
              steps { 
                 aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: '', onDisallowed: 'fail', outputFormat: 'html'
              }
         }  
         stage('Create EC2 Instance') {
              steps { 
                 ansiblePlaybook playbook: 'main.yaml', inventory: 'inventory'
              }
         }  
         stage('Upload to AWS') {
              steps {
                 withAWS(region:'us-east-2',credentials:'aws-static') {
                 sh 'echo "Uploading content with AWS creds"'
                     s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'course-1-moataz')
                 }
             }
         }      
     }
}

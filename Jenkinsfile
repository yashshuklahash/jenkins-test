pipeline {
    agent any

    environment {
        def config = readJSON file: 'app.json'
        project = "${config.Project_Name}"
        author = "${config.Author}"
        s3 = "${config.S3_Bucket}"
        app = "${config.Application}"
        api = "${config.API}"
    }
   
   options([
   parameters([
    string(name: 'Project Name', defaultValue: project , description: 'Enter your project name : ' )
   ])
       
       skipStagesAfterUnstable()
   ]) 
    

    
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Checkout Stage') {
            steps {
                sh 'echo "this is a checkout stage"'
                checkout scm
            }
        }
        stage('Build Stage') {
            steps {
                sh 'ls -la'
                
                echo project
                echo author
                echo s3
                echo app
                echo api
                
                sh 'echo "this is a build stage"'
                input "Does the staging environment look ok?"
            }
        }
        
        stage('Approval !! ') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        
        stage('Production Stage') {
            steps {
                sh 'echo "this is a Production Stage"'
            }
        }
    }
}

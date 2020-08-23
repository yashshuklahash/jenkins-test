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


    
    stages {
        /* "Build" and "Test" stages omitted */
        stage('Checkout Stage') {
            steps {
                sh 'echo "this is a checkout stage"'
                checkout scm
            }
        }
        
        stage ('Configure Pipeline Job')
        {
            input {
            message "Should we continue?"
            ok "Yes, do it!"
            parameters {
                string(name: 'Project Name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                string(name: 'Project Name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                string(name: 'Project Name', defaultValue: env.project , description: 'Enter your project name : ' )
            }
           
            }
        }
        
        stage('Build Stage') {
            steps {
               // script {
  
                   // env.PROJECT_NAME = input message: 'Please enter the project name ', ok: 'Validate!',
                      //  parameters: [string(name: 'Project Name', defaultValue: env.project , description: 'Enter your project name : ' )]
               // }
                sh 'ls -la'
                
                echo PROJECT_NAME
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

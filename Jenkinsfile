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
        
        stage('Configure Pipeline Job')
        {
            steps{
                env.configuration = input message: 'Please enter the pipeline configuration !', ok: 'Validate!', 
                    parameters: [string(name: 'Project_Name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                                string(name: 'Author', defaultValue: env.author , description: 'Enter your project name : ' ) ,
                                string(name: 'S3_Bucket_URL', defaultValue: env.project , description: 'Enter your project name : ' )]
            
        }
        }
        
        stage('Build Stage') {
            steps {
               // script {
  
                   // env.PROJECT_NAME = input message: 'Please enter the project name ', ok: 'Validate!',
                      //  parameters: [string(name: 'Project Name', defaultValue: env.project , description: 'Enter your project name : ' )]
               // }
                sh 'ls -la'
                
                echo configuration
                echo Author
                echo S3_Bucket_URL
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

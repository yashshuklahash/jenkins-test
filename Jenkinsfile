pipeline {
    agent any
    
    options {
    skipStagesAfterUnstable()
    }
    
    environment {
        def config = readJSON file: 'app.json'
        project = "${config.Project_Name}"
        author = "${config.Author}"
        s3 = "${config.S3_Bucket}"
        app = "${config.Application}"
        api = "${config.API}"
    }
    
    stages {
        
        stage('Checkout Stage') {
            steps {
                sh 'echo "this is a checkout stage"'
                checkout scm
            }
        }
        
        stage('Configure Pipeline Job'){
            steps{
                script {
                env.configuration = input message: 'Please enter the pipeline configuration !', ok: 'Validate!', 
                    parameters: [string(name: 'Project_Name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                                string(name: 'Author', defaultValue: env.author , description: 'Enter your project name : ' ) ,
                                string(name: 'S3_Bucket_URL', defaultValue: env.project , description: 'Enter your project name : ' )]
                }
        }
        }
        
        stage('Build Stage') {
            steps {    
                echo configuration
                echo configuration['Project_Name']
                echo configuration['Author']
                echo configuration['S3_Bucket_URL']
                sh 'echo "this is a build stage"'
            }
        }
        
        stage('Approval !! ') {
            steps {
                input "Deploy the code?"
            }
        }
        
        stage('Production Stage') {
            steps {
                sh 'echo "this is a Production Stage"'
            }
        }
    }
}

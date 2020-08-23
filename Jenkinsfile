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
        stage = "${config.Stage}"
        api = "${config.API}"
        stage_choice = "${config.Stage_choices}"
        highavailable = "${config.HA}"
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
                def configuration = input message: 'Please enter the pipeline configuration !', ok: 'Validate!', 
                    parameters: [string(name: 'name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                                 string(name: 'author', defaultValue: env.author , description: 'Enter script author name : ' ) ,
                                 string(name: 's3', defaultValue: env.S3_Bucket , description: 'Enter S3 bucket URL : ' )   ,
                                 string(name: 'api', defaultValue: env.API , description: 'Enter api endpoint : ' )  ,
                                 choice(name: 'stage', defaultValue: env.stage , choices: env.stage_choice , description: 'Enter stage to deploy to : ' )
                                 booleanParam(name: 'HA', defaultValue: env.highavailable , description: 'deploy in High Availability ? ' )]
                
                env.Project_Name = configuration.name
                env.Author_Name = configuration.author
                env.S3_Bucket_URL = configuration.s3
                env.API_Endpoint = configuration.api
                env.Stage = configuration.stage
                env.HA = configuration.HA
                
                 
                }
        }
        }
        
        stage('Build Stage') {
            steps {    
                
                echo env.Project_Name
                echo env.Author_Name 
                echo env.S3_Bucket_URL 
                echo env.API_Endpoint 
                echo env.Stage 
                echo env.HA 
                
            }
        }
        
        stage('Approval !! ') {
            steps {
                input message : "Deploy the code?" , ok: ' Deploy !'
            }
        }
        
        stage('Production Stage') {
            steps {
                sh 'echo "this is a Production Stage"'
            }
        }
    }
}

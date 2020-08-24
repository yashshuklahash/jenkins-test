def customers = ["Customer1", "Customer2", "Customer3"]

def performDeploymentStages(config,app) {
    stage("build") {
        
        echo "this is ${config.Project_Name}"
        
        
        
        
        
    }

}



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
                script{
                def configuration = input message: 'Please enter the pipeline configuration !', ok: 'Validate!', 
                    parameters: [string(name: 'Project_Name', defaultValue: env.project , description: 'Enter your project name : ' ) ,
                                 string(name: 'Script_Author', defaultValue: env.author , description: 'Enter script author name : ' ) ,
                                 string(name: 'S3_Bucket_URL', defaultValue: env.s3 , description: 'Enter S3 bucket URL : ' )   ,
                                 string(name: 'API_Endpoint', defaultValue: env.API , description: 'Enter api endpoint : ' )  ,
                                 choice(name: 'Stage', defaultValue: env.stage , choices: env.stage_choice , description: 'Enter stage to deploy to : ' ),
                                 booleanParam(name: 'High_Available', description: 'deploy in High Availability ? ' )]
                
                env.Project_Name = configuration.Project_Name
                env.Author_Name = configuration.Script_Author
                env.S3_Bucket_URL = configuration.S3_Bucket_URL
                env.API_Endpoint = configuration.API_Endpoint
                env.Stage = configuration.Stage
                env.HA = configuration.High_Available
                
                 
                }
        }
        }
        
        stage('Build Stage') {
            steps {    
                
                echo "Project Name is : $Project_Name "
                echo "Author Name is : $Author_Name "
                echo "S3 Bucket URL is : $S3_Bucket_URL " 
                echo "API Endpoint is : $API_Endpoint " 
                echo "Stage for Deployment is : $Stage " 
                echo "Is Deployment HighAvailale ? : $HA " 
                
            }
        }
        
        stage('Approval !! ') {
            steps {
                input message : "Approval! Deploy the code?" , ok: ' Deploy !'
            }
        }
        
        stage('Deploy To Production') {
           steps {
                script {
                   def nodes = [:]
                   for (cust in customers) {
                        def config = readJSON file: 'app.json'
                       echo "${config[cust][Prod]}"
                       nodes[cust] = {performDeploymentStages( ${config[cust][Prod]} ,cust)}
                   }   
                        parallel nodes
                }
           }
        }
    }
}

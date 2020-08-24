


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
                script{
                
                def customers = ["Customer1", "Customer2", "Customer3"]

                def parallelStagesMap = customers.collectEntries {
                                    ["${x}" : generateStage(x)]
                }

def generateStage(cust) {
    return {
        stage("${cust} : Deploy") {
            steps{ 
                script{
                    def config = readJSON file: 'app.json'            
                    def configuration = input message: 'Please enter the pipeline configuration !', ok: 'Validate!', 
                        parameters: [string(name: 'Project_Name', defaultValue: "${config.${cust}.Prod.Project_Name}" , description: 'Enter your project name : ' ) ,
                                    string(name: 'Script_Author', defaultValue: "${config.${cust}.Prod.Author}" , description: 'Enter script author name : ' ) ,
                                    string(name: 'S3_Bucket_URL', defaultValue: "${config.${cust}.Prod.S3_Bucket}" , description: 'Enter S3 bucket URL : ' )   ,
                                    string(name: 'API_Endpoint', defaultValue: "${config.${cust}.Prod.API}" , description: 'Enter api endpoint : ' )  ,
                                     choice(name: 'Stage' , choices: "${config.${cust}.Prod.Stage_choicee}" , description: 'Enter stage to deploy to : ' ),
                                    booleanParam(name: 'High_Available', defaultValue: "${config.${cust}.Prod.HA}"  ,  description: 'deploy in High Availability ? ' )]
                
                    env."${cust}_Project_Name" = configuration.Project_Name
                    env."${cust}_Author_Name" = configuration.Script_Author
                    env."${cust}_S3_Bucket_URL" = configuration.S3_Bucket_URL
                    env."${cust}_API_Endpoint" = configuration.API_Endpoint
                    env."${cust}_Stage" = configuration.Stage
                    env."${cust}_HA" = configuration.High_Available
                
                }
            
                echo "Project Name is : ${${cust}_Project_Name} "
                echo "Author Name is : ${${cust}_Author_Name} "
                echo "S3 Bucket URL is : ${${cust}_S3_Bucket_URL} " 
                echo "API Endpoint is : ${${cust}_API_Endpoint} " 
                echo "Stage for Deployment is : ${${cust}_Stage} " 
                echo "Is Deployment HighAvailale ? : ${${cust}_HA} "
            }               
        }
    }
}

                
                
                }    
                
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
                    parallel parallelStagesMap
                }
           }
        }
    }
}

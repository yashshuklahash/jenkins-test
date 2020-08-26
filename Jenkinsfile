// jenkins pipeline job
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
                
        stage('Build'){
            steps{
                echo "This is build stage"
                sh "sleep 5"
            }
        }
        
        stage('Dev Stage') {
            steps { 
                input message : "deploy to Dev ? " , ok : "Approve !"
                script{
                    def config = readJSON file: 'app.json'  
                    echo "Project Name is : ${config.Project_Name} "
                    echo "Author Name is : ${config.Author}"
                    echo "S3 Bucket URL is : ${config.S3_Bucket} " 
                    echo "API Endpoint is : ${config.API} " 
                    echo "Stage for Deployment is : ${config.Stage_choices} " 
                    echo "Is Deployment HighAvailale ? : ${config.HA}" 
                }
            }
        }
        
        
        stage('Customer Deploys'){
            parallel {
                stage('Customer 1') {
                    //def config = readJSON file: 'app.json'
                    
                    agent any
                        stages {
                            stage('Deploy to UAT') {
                                
                                steps {
                                     
                                     script{
                                     def stage = "UAT"
                                     def customer = "Customer1"
                                     def config = readJSON file: 'app.json'
                                     echo "Project Name is : ${config[customer][stage]["Project_Name"]}"
                                     echo "Author Name is : ${config[customer][stage]["Author"]} "
                                     echo "S3 Bucket URL is : ${config[customer][stage]["S3_Bucket"]} " 
                                     echo "API Endpoint is : ${config[customer][stage]["API"]} " 
                                     echo "Is Deployment HighAvailale ? : ${config[customer][stage]["HA"]}"
                                    }
                                }
                            }
                            stage('Deploy to Prod') {
                                when {
                                      branch 'master'
                                }                             
                                
                                steps {
                                    script{
                                     def stage = "Prod"
                                     def customer = "Customer1"
                                     def config = readJSON file: 'app.json'
                                     input message : 'Deploy To Prod ?' , ok : "Approve !"
                                     echo "Project Name is : ${config[customer][stage]["Project_Name"]}"
                                     echo "Author Name is : ${config[customer][stage]["Author"]} "
                                     echo "S3 Bucket URL is : ${config[customer][stage]["S3_Bucket"]} " 
                                     echo "API Endpoint is : ${config[customer][stage]["API"]} " 
                                     echo "Is Deployment HighAvailale ? : ${config[customer][stage]["HA"]}"
                                    }
                                }
                                 
                            }
                    }
            }
            stage('Customer 2') {
                agent any
               // def config = readJSON file: 'app.json'
                               
                stages {
                    stage('Deploy to UAT') {
                        
                        steps {
                            
                            script{
                            def customer = "Customer2" 
                            def stage = "UAT"
                            def config = readJSON file: 'app.json'
                            echo "Project Name is : ${config[customer][stage]["Project_Name"]}"
                            echo "Author Name is : ${config[customer][stage]["Author"]} "
                            echo "S3 Bucket URL is : ${config[customer][stage]["S3_Bucket"]} " 
                            echo "API Endpoint is : ${config[customer][stage]["API"]} " 
                            echo "Is Deployment HighAvailale ? : ${config[customer][stage]["HA"]}"
                            }
                         }
                    }
                    stage('Deploy to Prod') {
                        when {
                            anyOf { branch 'master'; branch 'Jenkins-pipe-test' }
                        }
                        
                        steps {
                            script{
                            def stage = "Prod"
                            def customer = "Customer2" 
                            def config = readJSON file: 'app.json'
                            input message : 'Deploy To Prod ?' , ok : "Approve !"
                            echo "Project Name is : ${config[customer][stage]["Project_Name"]}"
                            echo "Author Name is : ${config[customer][stage]["Author"]} "
                            echo "S3 Bucket URL is : ${config[customer][stage]["S3_Bucket"]} " 
                            echo "API Endpoint is : ${config[customer][stage]["API"]} " 
                            echo "Is Deployment HighAvailale ? : ${config[customer][stage]["HA"]}"
                            }
                        }
                    }
                    stage('Automated tests') {
                        when {
                            anyOf { branch 'master'; branch 'Jenkins-pipe-test' }
                        }
                        steps {
                            sh 'echo "Testing"'
                        }
                    }
                }
            }
        }
    }
     
    }
}

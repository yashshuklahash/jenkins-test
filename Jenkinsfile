// For Every customer a separate parallel stage is configured
def performDeploymentStages(config,customer,stage) {
    return{ 
        echo "this is ${stage} stage"
        echo "Project Name is : ${config[customer][stage]["Project_Name"]}"
        echo "Author Name is : ${config[customer][stage]["Author"]} "
        echo "S3 Bucket URL is : ${config[customer][stage]["S3_Bucket"]} " 
        echo "API Endpoint is : ${config[customer][stage]["API"]} " 
        echo "Is Deployment HighAvailale ? : ${config[customer][stage]["HA"]}"
        sh "sleep 5"
    }
}


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
        
        stage('Deploy : Test Stage') {
            steps {    
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
        
        stage('Approval !! Deploy : UAT ') {
            steps {
                input message : "Approval! Deploy to UAT?" , ok: ' Deploy To UAT !'
            }
        }
        
        stage('Deploy : UAT Stage') {
            steps {
                script {
                    def stage = "UAT"
                    def config = readJSON file: 'app.json'
                    def customers = config["customers"]
                    def parallelStagesMap = customers.collectEntries {
                        ["${it} : Deploy" : performDeploymentStages(config, it, stage)]
                    }
                parallel parallelStagesMap
                }
            }
        }

        stage('Approval !! Deploy : Production ') {
            steps {
                input message : "Approval! Deploy to Production?" , ok: ' Deploy To Production !'
            }
        }        

        stage('Deploy : Production Stage') {
            steps {
                script {
                    def stage = "Prod"
                    def config = readJSON file: 'app.json'
                    def customers = config["customers"]
                    def parallelStagesMap = customers.collectEntries {
                        ["${it} : Deploy" : performDeploymentStages(config, it, stage)]
                    }
                parallel parallelStagesMap
                }
            }
        }
    }
}

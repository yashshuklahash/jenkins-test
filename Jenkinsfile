pipeline {
    agent any
     options {
        skipStagesAfterUnstable()
    }
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
                script
                 {
                export name = readJSON file: 'app.json'
                }
                sh 'echo $name'
                
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

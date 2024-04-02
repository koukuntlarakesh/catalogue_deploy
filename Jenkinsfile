pipeline {
   agent {
    node {
        label 'agent1'
    }
    }
    options {
        timeout(time: 1, unit: 'HOURS') 
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment {
        packageVersion = ''
        nexusUrl= "172.31.92.65:8081"

        }
    
    parameters {
      string(name: 'version', defaultValue: '1.0.0', description: 'Version of catalogue?')
      string(name: 'environment', defaultValue: 'dev', description: 'Environment of  deployment (dev/prod) ?')

    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('getting version') {
            steps {
                script // we are script because this is ruby scripting
                {
                    echo "package  version :  ${params.version}"
                    echo "package  enviromnent :  ${params.environment}"
                }
            }
        
        }
        stage('Deploy') {
            steps {
                sh """ 
                 terraform init -reconfigure -backend-config = "${params.environment}/backend.tf"
                 """
            }
            steps {
                sh """ 
                 terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version = ${params.version} 
                 """
            }
            // steps {
            //     sh """ 
            //      terraform apply -auto-approve -var-file=${params.environment}/${params.environment}.tfvars -var="app_version = ${params.version} 
            //      """
            // }
        }
        
    }
     post { 
        always { 
            echo 'I will always say Hello again!'
             deleteDir()
        }
     }
        
    
    }
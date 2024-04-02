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
    // environment {
    //     packageVersion = ''
    //     nexusUrl= "172.31.92.65:8081"

        //}
    
    parameters {
      string(name: 'version', defaultValue: '', description: 'Version of catalogue?')
      string(name: 'environment', defaultValue: 'dev', description: 'Environment of  deployment (dev/prod) ?')
      booleanParam(name: 'Create', defaultValue: false, description: 'Want to create the infra?')
      booleanParam(name: 'Destroy', defaultValue: false, description: 'Want to delete the infra?')
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
        stage('init') {
            steps {
                sh """ 
                 cd terraform 
                 terraform init --backend-config=${params.environment}/backend.tf -reconfigure
                 """
            }
        }
        stage("plan"){
            when{
                expression
                {
                    params.Create
                }
            }
            steps {
                sh """ 
                cd terraform
                 terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
                 """
            }
        }
         stage("Apply"){
            when{
                expression
                {
                    params.Create
                }
            }
            steps {
                sh """ 
                cd terraform
                 terraform apply -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                 """
            }
        }
         stage("Destroy"){
            when{
                expression
                {
                    params.Destroy
                }
            }
            steps {
                sh """ 
                cd terraform
                 terraform destroy -var-file=${params.environment}/${params.environment}.tfvars -var="app_version = ${params.version} -auto-approve
                 """
            }
        }
    }
     post { 
        always { 
            echo 'I will always say Hello again!'
             deleteDir()
        }
        success
        {
            echo 'The excution is success'
        }
        failure
        {
            echo 'The is failure check the  logs'
        }
     
     }
        
    
    }
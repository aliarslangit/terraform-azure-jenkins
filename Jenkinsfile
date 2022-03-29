pipeline {
   agent any
    // environment {
    //     AZURE_SUBSCRIPTION_ID= credentials('azuresubid') 
    //     AZURE_TENANT_ID= credentials('azuretenantid')
    //     AZURE_CLIENT_ID= credentials('azureclientid')
    //     AZURE_CLIENT_SECRET= credentials('azureclientsecret')
    //     AZ_USERNAME=credentials('azusername')
    //     AZ_PASSWORD=credentials('azpassword')
    // }
    // parameters {
       
    //     string(name: 'clustername', defaultValue: 'demoaks', description: '')
    //     string(name: 'rgname', defaultValue: 'rg-demo-aks', description: '')
    //     string(name: 'location', defaultValue: 'eastus', description: '')
    //     string(name: 'nodesize', defaultValue: 'Standard_D2_v2', description: '')
    //     string(name: 'mincount', defaultValue: '1', description: '')
    //     string(name: 'maxcount', defaultValue: '3', description: '')
    //     string(name: 'dnsprefix', defaultValue: 'kubernetes-dns', description: '')
    //     string(name: 'owner', defaultValue: 'demo', description: '')
    //     string(name: 'environment', defaultValue: 'Dev', description: '')
    // }
    stages {
        stage('Git checkout') { 
            steps{
                sh 'whoami'
                git branch: 'main', credentialsId: 'ali123', url: 'https://github.com/aliarslangit/terraform-azure-jenkins.git'
        }
        }
  /*       stage('checkov') { 
            steps{
                container('checkov'){
                sh "checkov -d . -o junitxml > result.xml || true"
                junit "result.xml"
        }
        }
         }*/
             stage('Installing Azure Modules') {
            steps {
                    sh 'curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -'
                    sh 'sudo apt-add-repository "deb [arch=$(dpkg --print-architecture)] https://apt.releases.hashicorp.com $(lsb_release -cs) main"'
                    sh 'sudo apt install terraform'
                }
        }
        stage('Terraform Initialization') { 
            /*    when {
                expression {
                    params.destroy==false
                }
            }*/
            steps{
                   sh 'terraform init'
    }
        }
        stage('Check Terraform plan') { 
            steps {

              // sh 'terraform plan -var clustername=$clustername -var rgname=$rgname -var location=$location -var nodesize=$nodesize -var mincount=$mincount -var maxcount=$maxcount -var dnsprefix=$dnsprefix -var owner=$owner -var environment=$environment'
                  sh 'terraform plan'
     
            }
        }
        stage('Apply the terraform code') {
            /*when {
                expression {
                    params.destroy==false
                }
            }*/
            steps{
         
              //  sh 'terraform apply' 
                
                sh 'terraform apply -var clustername=$clustername -var rgname=$rgname -var location=$location -var nodesize=$nodesize -var mincount=$mincount -var maxcount=$maxcount -var dnsprefix=$dnsprefix -var owner=$owner -var environment=$environment -auto-approve'   
            /*    sh 'terraform apply $TWORKSPACE.out'
                script {
                EKSNAME = sh (
                script: 'terraform output EKSclustername',
                returnStdout: true).trim()
                echo "${EKSNAME}"
               }
               script {
                EKSCLUSTERAUTOSCALERARN = sh (
                script: 'terraform output EKSclusterautoscalerrole',
                returnStdout: true).trim()
                echo "${EKSCLUSTERAUTOSCALERARN}"
               } */
                  
            }
        }  
    
 }

 }
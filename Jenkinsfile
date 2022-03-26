pipeline {
     /*agent {
    kubernetes {
      label 'jenkins-slave'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins
  containers:
  - name: terraform
    image: hashicorp/terraform:0.14.8
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
  - name: helm-kubectl-azcli
    image: ams0/az-cli-kubectl-helm:latest
    command:
    - cat
    tty: true
  - name: checkov
    image: bridgecrew/checkov
    command:
    - cat
    tty: true
"""
}
  }*/
   agent {
    kubernetes {
      label 'jenkins-slave'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins
  containers:
  - name: terraform
    image: hashicorp/terraform:0.14.8
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
  - name: helm-kubectl-azcli
    image: aliarslandocker/azcli-helm-kubectl:V1 
    command:
    - cat
    tty: true  
  - name: checkov
    image: bridgecrew/checkov
    command:
    - cat
    tty: true
"""
}
  } 
    environment {
        AZURE_SUBSCRIPTION_ID= credentials('azuresubid') 
        AZURE_TENANT_ID= credentials('azuretenantid')
        AZURE_CLIENT_ID= credentials('azureclientid')
        AZURE_CLIENT_SECRET= credentials('azureclientsecret')
        AZ_USERNAME=credentials('azusername')
        AZ_PASSWORD=credentials('azpassword')
    }
    parameters {
       
        string(name: 'clustername', defaultValue: 'cmpaks', description: '')
        string(name: 'rgname', defaultValue: 'rg-cmp-aks', description: '')
        string(name: 'location', defaultValue: 'eastus', description: '')
        string(name: 'nodesize', defaultValue: 'Standard_D2_v2', description: '')
        string(name: 'mincount', defaultValue: '1', description: '')
        string(name: 'maxcount', defaultValue: '3', description: '')
        string(name: 'dnsprefix', defaultValue: 'kubernetes-dns', description: '')
        string(name: 'owner', defaultValue: 'CMP', description: '')
        string(name: 'environment', defaultValue: 'Dev', description: '')
    }
    stages {
        stage('Git checkout') { 
            steps{
                sh 'whoami'
                git branch: 'main', credentialsId: 'ali123', url: 'https://github.com/aliarslan-graduate/AKS.git'
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
         
        stage('Terraform Initialization') { 
            /*    when {
                expression {
                    params.destroy==false
                }
            }*/
            steps{
                
                container('terraform'){
                   sh 'terraform init'
                  //  sh 'terraform workspace select $TWORKSPACE || terraform workspace new $TWORKSPACE'
        }
    }
        }
        stage('Check Terraform plan') { 
          /*  when {
                expression {
                    params.destroy==false
                }
            }*/
            steps {
                container('terraform'){
               sh 'terraform plan -var clustername=$clustername -var rgname=$rgname -var location=$location -var nodesize=$nodesize -var mincount=$mincount -var maxcount=$maxcount -var dnsprefix=$dnsprefix -var owner=$owner -var environment=$environment'

                  }  
            }
        }
        stage('Apply the terraform code') {
            /*when {
                expression {
                    params.destroy==false
                }
            }*/
            steps{
                container('terraform') {
              //  sh 'terraform apply' 
                
              //  sh 'terraform apply -var clustername=$clustername -var rgname=$rgname -var location=$location -var nodesize=$nodesize -var mincount=$mincount -var maxcount=$maxcount -var dnsprefix=$dnsprefix -var owner=$owner -var environment=$environment -auto-approve'   
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
    

        stage('Run Kubectl scripts') {
         /*   when {
                expression {
                    params.destroy==false
                }
            } */
            steps {
                container('helm-kubectl-azcli'){
                withCredentials([azureServicePrincipal('azurelogin')])
                        {
                            sh 'whoami'
                            sh 'bash scripts/packages.sh'
                            sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
                            sh 'az aks get-credentials --name $clustername --resource-group $rgname'

                             // sh 'bash kubebench/kubebench.sh'
                            //  sh 'bash ingress/ingress.sh'
                            //  sh 'bash scripts/kubecost.sh'
                            //  sh 'bash scripts/helm.sh'
                             // sh 'bash scripts/kube-dashboard.sh'
                           //   sh 'bash scripts/opa.sh'
                            //sh 'bash az-ingress/ingress.sh'
                              sh 'bash updated-ingress/ingress.sh'
                          
                          //  sh "bash scripts/kubectl.sh $clustername $rgname"
                              
                           // sh 'bash scripts/ingress.sh'
                              
                          
                              
                       
                              
                                                  
                       }
                
             }      
            }
 }

 }}/*
        stage('Destroy the Infrastructure created by Terraform'){
            when {
                expression {
                    params.destroy
                }
            }
            steps{
                container(terraform) {
                sh 'terraform workspace select $TWORKSPACE'
                sh 'terraform init'
                sh 'terraform destroy -auto-approve'
            }
        }
        }
    }
}*/

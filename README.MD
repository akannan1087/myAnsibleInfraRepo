Pre-requisites:

Ansible is installed and Boto is also installed on Jenkins instance
Ansible plug-in is installed in Jenkins. 
Make sure you create an IAM role with AmazonEC2FullAccess policy and attach the role to Jenkins EC2 instance.
Playbook for creating new EC2 instance needs to be created but you can refer my GitHub Repo

please refer my website for step by step instructions - https://www.coachdevops.com/2022/12/automate-ec2-provisioning-in-aws-using.html

Create Ansible playbook for provisioning EC2 instance

(Sample playbook is available in my GitHub Repo, you can use that as a reference)

Create Jenkins Pipeline 
pipeline {
    agent any
    stages {
        stage ("checkout") {
            steps {
                  checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [],                                                     userRemoteConfigs: [[url: 'https://github.com/akannan1087/myAnsibleInfraRepo']]])         
            }
        }
        stage('execute') {
            steps {
                //to suppress warnings when you execute playbook    
                sh "pip install --upgrade requests==2.20.1"
                // execute ansible playbook
                ansiblePlaybook playbook: 'create-EC2.yml'
            }
        }
    }
}

Execute Pipeline

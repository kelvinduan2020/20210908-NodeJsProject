node{
    stage('Git Checkout'){
        git 'https://github.com/kelvinduan2020/20210908-NodeJsProject.git'
    }
    
    stage('Create Docker Image'){
        ansiblePlaybook installation: 'ansible', 
                        inventory: 'host.inv', 
                        playbook: 'CreateDockerImage.yml'
    }
    
    withCredentials([string(credentialsId: 'docker-hub-password', variable: 'dockerHubPWD')]) {
        stage('Push to DockerHub'){
            ansiblePlaybook credentialsId: 'ansible-to-webapp', 
                            installation: 'ansible', 
                            inventory: 'host.inv',
                            playbook: 'PushToDockerHub.yml'
        }
    }
    
    stage('Run On Container'){
        ansiblePlaybook credentialsId: 'ansible-to-webapp', 
                        installation: 'ansible', 
                        inventory: 'host.inv', 
                        playbook: 'DeployDockerImage.yml'
    }
}

node{
    stage('Git Checkout'){
        git branch: 'main', url: 'https://github.com/kelvinduan2020/20210908-NodeJsProject.git'
    }
    
    stage('Create Docker Image'){
        ansiblePlaybook credentialsId: 'ansible-to-webapp',
                        installation: 'ansible', 
                        inventory: 'ansible/host.inv', 
                        playbook: 'ansible/CreateDockerImage.yml'
    }
    
    withCredentials([string(credentialsId: 'docker-hub-password', variable: 'dockerHubPWD')]) {
        stage('Push to DockerHub'){
            ansiblePlaybook credentialsId: 'ansible-to-webapp', 
                            installation: 'ansible', 
                            inventory: 'ansible/host.inv',
                            playbook: 'ansible/PushToDockerHub.yml',  
                            extras: "-e docker_hub_pwd=${dockerHubPWD}"
        }
    }
    
    stage('Run On Container'){
        ansiblePlaybook credentialsId: 'ansible-to-webapp', 
                        installation: 'ansible', 
                        inventory: 'ansible/host.inv', 
                        playbook: 'ansible/DeployOnContainer.yml'
    }
}

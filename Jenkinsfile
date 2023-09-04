pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-pranay', url: 'https://github.com/kuntapranay/CI-usingAnsible.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // def mavenHome = tool name: 'Maven', type: 'Tool'
                    // sh "${mavenHome}/bin/mvn clean install"
                    sh "mvn clean package"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Clone your GitHub repository
                    git 'https://github.com/kuntapranay/CI-usingAnsible.git'
                    
                    // Run Ansible playbook from the cloned repository
                    ansiblePlaybook(
                        //playbook: 'CI-usingAnsible/deploy.yml', // Path to your deploy.yml in the cloned repo
                        playbook: '/var/lib/jenkins/workspace/CICDAnsible/deploy.yml', 
                        //inventory: 'path/to/ansible/inventory',
                        inventory: '/var/lib/jenkins/workspace/CICDAnsible/inventory.ini',
                        credentialsId: 'ansible-credentials'
                        //extras: '-e "variable=value"' // You can pass extra variables to your Ansible playbook
                    )
                }
            }
        }
    }

    post {
        success {
            //slackSend(color: 'good', message: "Deployment successful!") // Optionally notify on success
            echo 'Deployment succeeded. Sending notifications...'
        }
        failure {
            slackSend(color: 'danger', message: "Deployment failed!") // Optionally notify on failure
            echo 'Deployment failed. Sending notifications...'
        }
    }
}


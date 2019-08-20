pipeline {
    agent {
        label 'ansible'
    }

    options{
        buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '1'))
    }

    stages {
        
        stage("Build image"){
            steps {
                sh '''
                    docker build ./myapp-build -t marcusvmesquita/myapp
                '''
            }
        }

        stage("Test image"){
            steps {
                sh '''
                    docker run --rm -d -p 8080:80 marcusvmesquita/myapp
                    
                    sleep 20
                    
                    [[ `curl http://localhost:8080` == true ]] && echo "Success" || Failed
                '''
            }
        }
        
        stage("Deploy myapp via Ansible using playbook myapp-deploy.yml") {
            steps {
                sh '''
                    ansible-playbook myapp-deploy.yml --extra-vars 'target_hosts=10.171.230.7'
                '''
            }
        }
    }
}
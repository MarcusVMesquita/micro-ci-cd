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
                    docker build ./myapp-build -t marcusvmesquita/myapp:latest
                '''
            }
        }

        stage("Test image"){
            steps {
                sh '''
                    docker run --rm -d --name myapp-test -p 8099:80 marcusvmesquita/myapp:latest
                    
                    sleep 5
                    curl http://localhost:8099

                    if [ $? -eq 0 ]; then 
                        
                        echo "Test executed with success"
                        docker tag marcusvmesquita/myapp:latest marcusvmesquita/myapp:1.0
                        docker kill myapp-test
                        exit 0
                    
                    else
                    
                        echo "An error occured while testing marcusvmesquita/myapp"
                        docker kill myapp-test
                        exit 1
                    
                    fi
                '''
            }
        }

        stage("Push image to Docker Hub"){
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'docker_hub_credentials',
                    usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']
                ]){
                    sh '''
                        docker login -u $USERNAME -p $PASSWORD
                        docker push marcusvmesquita/myapp:1.0
                        docker push marcusvmesquita/myapp:latest

                    '''
                }
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
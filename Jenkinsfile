pipeline {
    agent {
        label 'ansible'
    }

    options{
        buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '1'))
    }

    stages {
        stage("Deploy myapp via Ansible using playbook myapp-deploy.yml") {
            steps {
                sh '''
                    ansible-playbook myapp-deploy.yml --extra-vars 'target_hosts=10.171.230.7'
                '''
            step([$class: 'hudson.plugins.chucknorris.CordellWalkerRecorder'])

            }
        }
    }
}
pipeline {
    agent {
        label 'ansible'
    }

    // triggers {
    //     pollSCM('* * * * *')
    // }

    options{
        buildDiscarder(logRotator(numToKeepStr: '10', daysToKeepStr: '1'))
    }

    stages {
        stage("Deploy myapp via Ansible using playbook myapp-deploy.yml") {
            steps {
                sh '''
                    ansible-playbook myapp-deploy.yml
                '''
            step([$class: 'hudson.plugins.chucknorris.CordellWalkerRecorder'])

            }
        }
    }
}
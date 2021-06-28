pipeline {
    agent {
        label "ansible-docker"
    }
    
    stages
    {
        stage("clone") {
            steps {
                git credentialsId: "44de766a-8add-4112-ac39-6bb223138b79", url: "git@github.com:shhhowtime/example-playbook.git"
                sh "ssh-keyscan github.com >> ~/.ssh/known_hosts"
                withCredentials([sshUserPrivateKey(credentialsId: '44de766a-8add-4112-ac39-6bb223138b79', keyFileVariable: 'GITKEY', passphraseVariable: 'GITPASS', usernameVariable: 'GITUSER')]) 
                {
                    sh 'cp -f $GITKEY ~/.ssh/id_rsa'
                }
                sh "ansible-galaxy install -r requirements.yml -p roles"
            }
        }
        stage("run")
        {
            steps
            {
                ansiblePlaybook (
                    playbook: 'site.yml',
                    inventory: 'inventory/prod.yml')
            }
        }
    }
}

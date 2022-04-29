pipeline {
  agent any
    parameters {
  choice choices: ['qa', 'production'], description: 'Select environment for deployment', name: 'DEPLOY_TO'

    string(name: 'upstreamJobName',
          defaultValue: '',
          description: 'The name of the job the triggering upstream build'
    )
}
  
  stages {
    stage('Copy artifact') {
      steps {
        archiveArtifacts artifacts: 'main.rb', followSymlinks: false
      }
    }
    stage('Deliver') {
      steps {
            withCredentials([sshUserPrivateKey(credentialsId: "vagrant-private-key", keyFileVariable: 'keyfile')]) {
                     sh 'ansible-playbook --private-key=${keyfile} -i hosts.ini playbook.yml'
            // sh 'scp -o "StrictHostKeyChecking=no" -i ${keyfile} ./sample vagrant@10.10.50.3:'
        }
      }
    }
  }
}

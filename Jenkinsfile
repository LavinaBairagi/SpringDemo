pipeline {
  agent any
  stages {
    stage('Git-checkout') { // for display purposes
      when {
        not {
          branch 'master'
        }
      }
      steps {
        git branch: env.BRANCH_NAME,
          url: 'https://github.com/LavinaBairagi/SpringDemo.git'
      }
    }

    stage('Build') {
      when {
        anyOf {
          branch: {env.BRANCH_NAME/*}
        }
      }
      steps {
        sh "mvn clean test"
      }
    }

    stage('Package') {
      when {
        anyOf {
          branch 'env.BRANCH_NAME/*'
        }
      }
      steps {
        sh "mvn package"
      }
    }

    stage('Nexus') {
      when {
        anyOf {
          branch 'env.BRANCH_NAME/*'
        }
      }
      steps {
        nexusArtifactUploader artifacts: [
            [artifactId: 'demo',
              classifier: '', file: 'target/demo-0.0.1-SNAPSHOT.jar',
              type: 'jar'
            ]
          ], credentialsId: 'nexus', groupId: 'com.example',
          nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3',
          protocol: 'http',
          repository: 'springdemo',
          version: '0.0.1'
      }
    }

    stage('ansible-deploy') {
      when {
        anyOf {
          branch 'env.BRANCH_NAME/*'
        }
      }
      steps {
        ansiblePlaybook(credentialsId: 'id_rsa', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'download.yml')
      }
    }
  }
}

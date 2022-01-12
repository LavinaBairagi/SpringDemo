pipeline {
  agent any
  environment {
  
    VERSION = readMavenPom().getVersion()
    }
   
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

    stage('pom') {
      steps {
        
        echo "${VERSION}"
        
        
      }
    }
    
    
    stage('Build') {
      steps {
        sh "mvn clean test"
      }
    }

    stage('Package') {
      steps {
        sh "mvn package"
         echo "${VERSION}"
      }
    }

    stage('Nexus') {

      steps {
        nexusArtifactUploader artifacts: [
            [artifactId: 'demo',
              classifier: '', file: 'target/demo-0.0.1-SNAPSHOT.jar',
              type: 'jar'
            ]
          ], credentialsId: 'nexus', groupId: 'com.example',
          nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3',
          protocol: 'http',
          repository: 'develop-snapshot',
          version: '0.0.1-SNAPSHOT'
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

node {
    stage('Preparation') { // for display purposes
    git branch: 'developer',
        credentialsId: 'Github',
        url: 'https://github.com/LavinaBairagi/SpringDemo.git'
        
    }
    stage('Var') {
        steps{
            echo "BUILD_NUMBER =${env.BUILD_NUMBER}"
            sh "echo BUILD_NUMBER= $BUILD_NUMBER"
        }
    }
   
    stage('Build') {
                sh "mvn clean test"
    }
    
    stage('Package') {
                sh "mvn package"
    }
   
    
    stage('Nexus') {
       nexusArtifactUploader artifacts: [[artifactId: 'demo',
 classifier: '', file: 'target/demo-0.0.1-SNAPSHOT.jar',
 type: 'jar']], credentialsId: 'nexus', groupId: 'com.example', 
nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3',
 protocol: 'http', 
repository: 'springdemo',
 version: '0.0.1'    }
 
    
    stage('ansible-deploy'){
        
ansiblePlaybook(credentialsId: 'id_rsa', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'download.yml')    
   }
 

}

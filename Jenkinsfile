node {
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
       git 'https://github.com/LavinaBairagi/nexus-jenkins.git'
        
    }
   
    stage('Build') {
                sh "mvn clean test"
    }
    
    stage('Package') {
                sh "mvn package"
    }
   
    
    stage('Nexus') {
       nexusArtifactUploader artifacts: [[artifactId: 'demo',
 classifier: '', file: 'target/spring-demo-0.0.1-SNAPSHOT.war',
 type: 'jar']], credentialsId: 'nexus', groupId: 'com.example', 
nexusUrl: 'host.docker.internal:8110', nexusVersion: 'nexus3',
 protocol: 'http', 
repository: 'springdemo',
 version: '0.0.1'    }
 
    
    stage('ansible-deploy'){
        
ansiblePlaybook(credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'download.yml')    
   }
 

}
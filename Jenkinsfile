node {
    stage('Preparation') { // for display purposes
    git branch: '${env.GIT_BRANCH}',
        credentialsId: 'Github',
        url: 'https://github.com/LavinaBairagi/SpringDemo.git'
        }
    
    stage('Build') {
                echo 'Pulling...' + env.BRANCH_NAME
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
 version: readMavenPom().getVersion()    }
 
    
    stage('ansible-deploy'){
        
ansiblePlaybook(credentialsId: 'id_rsa', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'download.yml')    
   }
 

}

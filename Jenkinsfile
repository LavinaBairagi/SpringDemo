node {
    stage('Preparation') { // for display purposes
    checkout([$class: 'GitSCM',
              branches: [[name: '*/master'], [name: '*/developer']],
              extensions: [],
              userRemoteConfigs: [[url: 'https://github.com/LavinaBairagi/SpringDemo.git']]])

        }
     stage('show branches'){
                 branchesList = readFile("branch.txt").trim()
                    echo branchesList // this line I want to get all branches

             // the Question is How to create an Array from branchesList
            
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
 version: '0.0.1'    }
 
    
    stage('ansible-deploy'){
        
ansiblePlaybook(credentialsId: 'id_rsa', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory.inv', playbook: 'download.yml')    
   }
 

}

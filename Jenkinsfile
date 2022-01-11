node {
    stage('Preparation') { // for display purposes
    git branch: '${env.GIT_BRANCH}',
        credentialsId: 'Github',
        url: 'https://github.com/LavinaBairagi/SpringDemo.git'
        }
    
    
    stage('show branches'){
            steps{
                 // Define Array containing branches
                  def BRANCHES = []
                 // Give your path of file. Please change the path according to your requirements

                 def branchesList = readFile(file: branch.txt')

                 // I assumed your file contains one branch per line, that is why i split with "\n".
                 // You can change this line as per your condition

                 BRANCHES = branchesList.split("\n")                 

                 for (branch in BRANCHES){
                          println ("Branch is : ${branch}") 
                       }
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

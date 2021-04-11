node {
  def remote = [:]
  remote.name = 'Ansible'
  remote.host = '172.31.4.250'
  remote.user = 'root'
  remote.password = 'phani@123'
  remote.allowAnyHosts = true
    stage('Compile'){
        sh 'mvn compile'
    }
    stage('test'){
        sh 'mvn test'
    }
    stage('package'){
        sh 'mvn package'
    }
    
    stage('send dockerfile'){
        sshPut remote: remote, from: '/var/lib/jenkins/workspace/Project/Dockerfile' , into: '/opt'
        sshPut remote: remote, from: '/var/lib/jenkins/workspace/Project/target/WebAppCal*.war' , into: '/opt'

      
    }
  stage('Remote SSH') {
      sshCommand remote: remote, command: "docker image build -t $JOB_NAME:v1.$BUILD_ID /opt/."
      sshCommand remote: remote, command: "docker image tag $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:v1.$BUILD_ID"
      sshCommand remote: remote, command: "docker image tag $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:latest"
      sshCommand remote: remote, command: "docker image push phani09/$JOB_NAME:v1.$BUILD_ID"
      sshCommand remote: remote, command: "docker image push phani09/$JOB_NAME:latest"
      sshCommand remote: remote, command: "docker image rmi $JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:v1.$BUILD_ID phani09/$JOB_NAME:latest"
  
  }
  
   
   
}




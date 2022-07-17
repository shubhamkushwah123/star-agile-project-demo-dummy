node{
    def mavenHome
    def mavenCMD
    def docker
    def dockerCMD
    
    stage('preparation for jenkins'){
        echo 'initializing variables and tools'
        mavenHome = tool name:'maven' , type: 'maven'
        mavenCMD = "${mavenHome}/bin/mvn"
        docker = tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        dockerCMD = "${docker}/bin/docker"
        
    }
    
    stage('git checkout'){
    try{
        echo 'checking out code from git repository'        
        git 'https://github.com/shubhamkushwah123/star-agile-project-demo.git'
    }
    catch(Exception e){
        echo "Exception occured in checking out the code from git repository"
        currentBuild.result="FAILURE"
        emailext body: '''Hi Team, 
        The Build has been failed. Request you to have a look and fix it asap.''', subject: 'Job ${JOB_NAME} (${BUILD_NUMBER} is failed', to: 'shubhamkushwah123@gmail.com'
    }
    }
    
    
    stage('clean package'){
        //sh 'mvn clean package'
         sh "${mavenCMD} clean package"
    }
    
    stage('Genenrating test reports '){
        echo "generating test reports .execute code quality and security analysis ...."
    }
    
    stage('building docker image'){
        echo 'Buidling docker image from docker file ....'
        sh "${dockerCMD} build -t shubhamkushwah123/addressbook:8.0 ."
    }
    
    stage('Login and Push to DockerHub'){
        echo "Logging in to the DockerHub and Pushing the image"
        withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHub')]) {
            sh "${dockerCMD} login -u shubhamkushwah123 -p ${dockerHub}"
            echo "pushing the image to the dockerHub..."
            sh "${dockerCMD} push shubhamkushwah123/addressbook:8.0"
        }
    }
    
    stage('Configure test server and deploy'){
        echo "configuring test server and deploy"
        //ansiblePlaybook credentialsId: 'ansible-ssh-connection', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-server.yml'
        ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-server.yml'
        
    }
    
    stage('execute selenium test suit'){
        echo 'executing selenium testcases'
        //command to run selenium application
    }
    
    
    stage('configure and deploy to production server'){
        echo 'deploying to production server'
        //ansible playbook to configure prod server and deploy application
    }
    stage('clear workspace'){
        echo 'cleaning up the workspace'    
        cleanWs()
        //send email
    }
    
}

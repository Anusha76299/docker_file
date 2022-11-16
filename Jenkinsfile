node{
    def buildNumber = BUILD_NUMBER
    stage("Git Clone"){
        
        git url: 'https://github.com/Anusha76299/docker_file.git', branch: 'main'
    }
    stage('Maven clean package'){
        
        def mavenHome= tool name: "maven", type: "maven"
        
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage("Build Docker Image"){
        
        sh "docker build -t anusha76299/web-app:${buildNumber} ."
    }
    stage("docker login and push"){
        withCredentials([string(credentialsId: 'admin', variable: 'admin')]) {
          sh "docker login -u anusha76299 -p ${admin}"
        }
         sh "docker push anusha76299/web-app:${buildNumber}"
    }
    stage("deploy application as docker"){
        //def dockerRun = "docker run -d -p 8080:8080 --name webappcontainer anusha76299/web-app:${buildNumber}"
        sshagent(['docke_dev']) {
        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.82.175.79 docker rm -f webappcontainer || true"
        sh "ssh -o StrictHostKeyChecking=no ec2-user@3.82.175.79 docker run -d -p 8080:8080 --name webappcontainer anusha76299/web-app:${buildNumber}"
        }
    }
}

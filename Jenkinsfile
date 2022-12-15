node{
    
   def buildNumber = BUILD_NUMBER
   
    stage("Git CheckOut"){
        git url: 'https://github.com/sibi51/java-web-app-docker.git',brach: 'master'
    }
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    }
    stage("Build Docker Image") {
         sh "docker build -t sibi51/java-web-app:${buildNumber} ."
    }
     stage("Docker Push"){
       withCredentials([string(credentialsId: 'sibi51', variable: 'Dockerpassword')]) {
             sh "docker login -u sibi51 -p${Dockerpassword}"
       }
    sh "docker push sibi51/java-web-app:${buildNumber}"
     }
     stage ("Docker run")
    sh "docker run -itd --name tomcat -p 9090:8080 sibi51/java-web-app:${buildNumber}"
}

node {
    
    stage("Git clone"){
        git credentialsId: 'Git-Credentials', url: 'https://github.com/MarutiD/Jenkins.git'
    }
    stage("Maven Build"){
        def mavenHome = tool name:"Maven-3.9.4", type: "maven"
        def mavenPath ="${mavenHome}/bin/mvn";
        sh "${mavenPath} clean package"
    }
    stage("Sonar Review"){
        withSonarQubeEnv(credentialsId: 'Sonar-Token') {
         def mavenHome = tool name: "Maven-3.9.4", type: "maven"
		 def mavenCMD = "${mavenHome}/bin/mvn"
			sh "${mavenCMD} sonar:sonar"
}
    }
    
    stage("Uploading Artifcat"){
        nexusArtifactUploader artifacts: [[artifactId: '0.1-SNAPSHOT', classifier: '', file: 'target/jenkins.war', type: 'war']], credentialsId: 'Nexus-Credentials', groupId: 'in.Maruti', nexusUrl: '3.110.187.36:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Maruti-SNAPSHOT-Repo', version: '1.0-SNAPSHOT'
    }
    stage("Tomcat Deploy to server"){
        sshagent(['03baafe9-86f7-4ab2-a80c-788741807dee']) {
        sh 'scp -o StrictHostKeyChecking=no target/jenkins.war ec2-user@3.108.217.26:/home/ec2-user/apache-tomcat-9.0.80/webapps'
}
    }
    
}
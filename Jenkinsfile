node {
   
  
    
     environment {
   
    def mavenHome = "/usr/share/maven"
    def javaHome = "/usr/lib/jvm/java-8-openjdk-amd64"
   def server = Artifactory.server 'jenkins-artifactory-server'
  }

    stage ('Clone') {
        git url: 'https://github.com/JFrog/project-examples.git'
    }

    stage ('Artifactory configuration') {
       
       server = Artifactory.server jenkins-artifactory-server
        Maven.tool = MAVEN_TOOL // Tool name from Jenkins configuration
        Maven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        Maven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        Maven.run pom: 'maven-example/pom.xml', goals: 'clean install', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}

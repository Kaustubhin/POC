 node{

      env.MAVEN_HOME = "${tool 'Maven 3.3.9'}"
      
      env.PATH="${env.MAVEN_HOME}/bin:${env.PATH}"
     

      def art_server = Artifactory.server 'artifactory_srvr'
      def rtMaven = Artifactory.newMavenBuild()
      def buildInfo = Artifactory.newBuildInfo()

      stage('Git checkout') {
           checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: 'https://github.com/Kaustubhin/POC.git']]])
      }
      stage('Artifactory Configuration') {
        rtMaven.tool = 'Maven 3.3.9'
          rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server:art_server
          rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server:art_server                
      }

      stage('Maven Build') {
          buildInfo.env.capture = true
          rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo
      }    

      stage('Artifact Publication') {
          art_server.publishBuildInfo buildInfo
      }

}

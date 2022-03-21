node{
  stage('compile'){
    git 'https://github.com/faizalgit/app1'
    def readcounter = readFile(file: 'versionInfo.txt')
    readcounter=readcounter.toInteger() +1
    def version= "Version" + readcounter
    println(version)
    sh 'mvn install -Dversion=' + "${version}"
    writeFile(file: 'versionInfo.txt', text:readcounter.toString())
  }
  stage('upload to nexus'){
   nexusArtifactUploader artifacts: [[artifactId: 'roshambo', classifier: '', file: 'target/roshambo-1.0-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus-upload', groupId: 'com.mcnz.rps', nexusUrl: '34.125.172.8:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexus-repo', version: '1.0'
  }
}

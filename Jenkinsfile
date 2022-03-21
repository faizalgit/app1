def version
node{
  stage('compile'){
    git 'https://github.com/faizalgit/app1'
    def readcounter = readFile(file: 'versionInfo.txt')
    readcounter=readcounter.toInteger() +1
    version= "Version" + readcounter
    println(version)
    sh 'mvn package -Dversion=' + "${version}"
    writeFile(file: 'versionInfo.txt', text:readcounter.toString())
  }
  stage('upload to nexus'){
    echo '-------------------------'
    echo 'upload to nexus begins..'
    println(version)
   nexusArtifactUploader artifacts: [[artifactId: 'roshambo', classifier: '', file: 'target/roshambo-'println(version)'.jar', type: 'jar']], credentialsId: 'nexus-upload', groupId: 'com.mcnz.rps', nexusUrl: '34.125.172.8:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexus-repo', version: '1.0'
  }
}

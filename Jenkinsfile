def version
node{
  stage('compile'){
    sh 'rm -r app1'
    git credentialsId: 'd8bd2da8-02f4-4142-9e3c-a01737564ed9', url: 'https://github.com/faizalgit/app1'
    sh 'git clone https://github.com/faizalgit/app1'
    sh 'git status'
    def readcounter = readFile(file: 'versionInfo.txt')
    readcounter=readcounter.toInteger() +1
    version= "Version" + readcounter
    println(version)
    sh 'mvn package -Dversion=' + "${version}"
    writeFile(file: 'versionInfo.txt', text:readcounter.toString())
    sh 'git status'
    sh 'git add versionInfo.txt'
    sh 'git commit -m "vertionInfo.txt updated and committed to Git"'
    sh 'git push origin master'
    
    }
  stage('upload to nexus'){
    echo '-------------------------'
    echo 'upload to nexus begins..'
    println(version)
   nexusArtifactUploader artifacts: [[artifactId: 'roshambo', classifier: '', file: 'target/roshambo-'+version+'.jar', type: 'jar']], credentialsId: 'nexus-upload', groupId: 'com.mcnz.rps', nexusUrl: '34.125.172.8:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexus-repo', version: version
  }
}

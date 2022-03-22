def version
node{
            
     stage('compile'){
     if (fileExists('app1')) {
       sh 'rm -r app1'
      }
    git credentialsId: 'FaizGit', url: 'https://github.com/faizalgit/app1'
    sh 'git clone https://github.com/faizalgit/app1'
    sh 'git status'
    sh 'mvn package -Dversion=' + "${version}"
    lastChanges()
  }
  
   stage('Version'){
    def readcounter = readFile(file: 'versionInfo.txt')
    readcounter=readcounter.toInteger() +1
    version= "Version" + readcounter
    println(version)
    writeFile(file: 'versionInfo.txt', text:readcounter.toString())
    sh 'git status'
    sh 'git add versionInfo.txt'
    sh 'git commit -m "vertionInfo.txt updated and committed to Git"'
      withCredentials([usernamePassword(credentialsId: 'FaizalGit',
                 usernameVariable: 'username',
                 passwordVariable: 'password')]){
                     sh('git push https://${username}:${password}@github.com/faizalgit/app1')
                 }
    }
    stage('upload to nexus'){
    echo '-------------------------'
    echo 'upload to nexus begins..'
    println(version)
   nexusArtifactUploader artifacts: [[artifactId: 'roshambo', classifier: '', file: 'target/roshambo-'+version+'.jar', type: 'jar']], credentialsId: 'nexus-upload', groupId: 'com.mcnz.rps', nexusUrl: '34.125.172.8:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexus-repo', version: version
  }
}

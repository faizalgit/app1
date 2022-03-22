def version
def readcounter
node{
     
    stage('compile'){
     if (fileExists('app1')) {
       sh 'rm -r app1'
      }
                
    git credentialsId: 'FaizGit', url: 'https://github.com/faizalgit/app1'
    sh 'git clone https://github.com/faizalgit/app1'
    sh 'git status'
    readcounter = readFile(file: 'versionInfo.txt')
    readcounter=readcounter.toInteger() +1
    version= "Version" + readcounter
    println(version)
    sh 'mvn package -Dversion=' + "${version}"
    lastChanges()
  }
   stage("last-changes") {
        def publisher = LastChanges.getLastChangesPublisher "PREVIOUS_REVISION", "SIDE", "LINE", true, true, "", "", "", "", ""
              publisher.publishLastChanges()
              def changes = publisher.getLastChanges()
              println(changes.getEscapedDiff())
              for (commit in changes.getCommits()) {
                  println(commit)
                  def commitInfo = commit.getCommitInfo()
                  
                  println(commitInfo)
                  echo 'start__modified__files'
                  println(commitInfo.getCommitMessage())
                  echo 'end__modified__files'
                  println(commit.getChanges())
              }
      }
  
   stage('Version'){
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

def version
def readcounter
def modifiedFiles
def skipBuild
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
   stage("Check for Code Change") {
                  if (fileExists('modifiedFiles.txt')) {
                    sh 'rm -r modifiedFiles.txt'
                  }
                  def publisher = LastChanges.getLastChangesPublisher "PREVIOUS_REVISION", "SIDE", "LINE", true, true, "", "", "", "", ""
                  publisher.publishLastChanges()
                  def changes = publisher.getLastChanges()
                  println(changes.getEscapedDiff())
                  for (commit in changes.getCommits()) {
                         println(commit)
                         def commitInfo = commit.getCommitInfo()
                         println(commitInfo)
                         println(commitInfo.getCommitMessage())
                         writeFile(file: 'modifiedFiles.txt', text:commitInfo.getCommitMessage())
                         modifiedFiles=readFile(file: 'modifiedFiles.txt').trim()
                         echo modifiedFiles
                         println(commit.getChanges())
                  }
                  
                  skipBuild="skip_build"
                  if (skipBuild.equals(modifiedFiles)) {
                             echo 'no code change detected..build will stop here'
                              sh 'false'                                  
                         } else {
                              writeFile(file: 'versionInfo.txt', text:readcounter.toString())
                              sh 'git status'
                              sh 'git add versionInfo.txt'
                              sh 'git commit -m "skip_build"'
                              withCredentials([usernamePassword(credentialsId: 'FaizalGit',
                              usernameVariable: 'username',
                              passwordVariable: 'password')]){
                              sh('git push https://${username}:${password}@github.com/faizalgit/app1')}
                              if (fileExists('modifiedFiles.txt')) {
                                   sh 'rm -r modifiedFiles.txt'
                              }
                   }
    }                           
    stage('upload to nexus'){
          echo '-------------------------'
          echo 'upload to nexus begins..'
          println(version)
          nexusArtifactUploader artifacts: [[artifactId: 'roshambo', classifier: '', file: 'target/roshambo-'+version+'.jar', type: 'jar']], credentialsId: 'nexus-upload', groupId: 'com.mcnz.rps', nexusUrl: '34.125.172.8:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexus-repo', version: version
    }
}

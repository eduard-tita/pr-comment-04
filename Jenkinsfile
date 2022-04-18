pipeline {
  agent any
  stages {
    stage("Copy artifacts") {
      steps {
        script {
          if (currentBuild.previousSuccessfulBuild) {
             copyArtifacts(projectName: currentBuild.projectName,
                           selector: specific("${currentBuild.previousSuccessfulBuild.number}"))
          }
        }
      }
    }
    stage("Check Jenkins") {
      steps {
        script {
          def versionFile = 'jenkins.txt'
          def lastVersion
          try {
              lastVersion = readFile(file: versionFile)
              echo("Last version was: ${lastVersion}")
          } catch(ignored) {
              echo("Last version was not known.")
          }

          if (!lastVersion) {
            // write it for the first time
            writeFile(file: versionFile, text: "${env.BUILD_NUMBER}")
          }
          else {
            // update it, if needed
            def latestVersion = latestAvailableVersion('https://www.jenkins.io/changelog-stable/rss.xml', '*')
            echo("Latest version was: ${latestVersion}")
          }
        }
      }
    }
    stage('Archive artifacts') {
      steps {
        archiveArtifacts artifacts: '*.txt'
      }
    }
  }
}

String latestAvailableVersion(String url, String regexp) {
  String version
  def response = httpRequest url
  println("Status: "+response.status)
  println("Content: "+response.content)  
  return version  
}

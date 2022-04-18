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
          String versionFile = 'jenkins.txt'
          String lastVersion
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
            String latestVersion = latestAvailableVersion('https://www.jenkins.io/changelog-stable/rss.xml', '<title>Jenkins ([^<]+)</title>')
            echo("Latest version was: ${latestVersion}")
            if (!latestVersion && latestVersion != lastVersion) {
              writeFile(file: versionFile, text: latestVersion)
            }
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

import java.util.regex.*

String latestAvailableVersion(String url, String regexp) {
  def response = httpRequest url  
  if (response.content) {
    Pattern pattern = Pattern.compile(regexp)
    Matcher matcher = pattern.matcher(response.content) 
    if (matcher.find()) {
      return matcher.group(1)
    }
  }
  return null
}

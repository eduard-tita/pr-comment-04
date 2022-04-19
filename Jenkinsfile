@Library(['int-jenkins-shared@INT-6671_Tracking_Upstream_Dependencies']) _

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
    stage("Jenkins") {
      steps {
        script {
          String latestVersion = checkUpstreamVersion(
              versionFile: 'jenkins.txt', 
              url: 'https://www.jenkins.io/changelog-stable/rss.xml', 
              regexp: '<title>Jenkins ([^<]+)</title>'
          )
          echo("Latest version returned: ${latestVersion}")
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

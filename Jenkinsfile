pipeline {
  agent any
  stages {
    stage("Copy artifacts") {
      steps {
        script {
          if (currentBuild.previousBuild) {
             copyArtifacts(projectName: currentBuild.projectName,
                           selector: specific("${currentBuild.previousBuild.number}"))
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
        }
      }
    }
    stage('Archive artifacts') {
      archiveArtifacts artifacts: '*.txt'
    }
  }
}

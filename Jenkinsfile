@Library(['int-jenkins-shared@INT-6671_Tracking_Upstream_Dependencies']) _

pipeline {
  agent any
  stages {
    stage("Copy artifacts") {
      steps {
        script {
          if (currentBuild.previousSuccessfulBuild) {
             copyArtifacts projectName: currentBuild.projectName,
                 selector: specific("${currentBuild.previousSuccessfulBuild.number}")
          }
        }
      }
    }
    stage("Jenkins") {
      steps {
        script {
          String latestVersion = checkUpstreamVersion versionFile: 'jenkins.txt', 
              url: 'https://www.jenkins.io/changelog-stable/rss.xml', pattern: '<title>Jenkins ([^<]+)</title>'
          if (latestVersion) {
            echo "Newer available version found: ${latestVersion}"
            Map payload = createPayload('Jenkins', latestVersion)
            def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
            echo "New Jira issue created: ${newIssue.data.key}"
          }
        }
      }
    }
    stage('Save artifacts') {
      steps {
        archiveArtifacts artifacts: '*.txt'
      }
    }
  }
}

static Map createPayload(String product, String version) {
  return [
      fields: [
          project: [key: 'TP'],
          issuetype: [name: 'Task'],
          summary: "Check ${product} plugin compatybility with ${product} ${version}",
          description: "h4. AC\\n" +
              " * The ${product} plugin works with ${product} ${version}\\n" +
              " * The ${product} plugin documentation is updated accordingly"
      ]
  ] as Map
}

@Library(['int-jenkins-shared']) _

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
          String latestVersion = checkUpstreamVersion versionFile: 'jenkins/lastVersion.txt',
              url: 'https://www.jenkins.io/changelog-stable/rss.xml',
              pattern: '<title>Jenkins ([^<]+)</title>'
          if (latestVersion) {
            echo "Newer available version found: ${latestVersion}"
            Map payload = createPayload('Jenkins plugin', 'Jenkins', latestVersion)
            def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
            echo "New Jira issue created: ${newIssue.data.key}"
          }
        }
      }
    }
    stage("Azure DevOps") {
      steps {
        script {
          String latestVersion = checkUpstreamVersion versionFile: 'azureDevOps/lastVersion.txt',
              url: 'https://docs.microsoft.com/en-us/azure/devops/server/download/azuredevopsserver?view=azure-devops',
              pattern: '<td>Azure DevOps Server (20[0-9\\.]+)</td>'
          if (latestVersion) {
            echo "Newer available version found: ${latestVersion}"
            Map payload = createPayload('Azure DevOps plugin', 'Azure DevOps', latestVersion)
            def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
            echo "New Jira issue created: ${newIssue.data.key}"
          }
        }
      }
    }


    stage('Save artifacts') {
      steps {
        archiveArtifacts artifacts: '**/*.txt'
      }
    }
  }
}

static Map createPayload(String plugin, String product, String version) {
  return [
      fields: [
          project: [key: 'TP'],
          issuetype: [name: 'Task'],
          summary: "Check ${plugin} compatybility with ${product} ${version}",
          description: """
h4. AC
 * The ${plugin} works with ${product} ${version}
 * The ${plugin} documentation is updated accordingly
"""
      ]
  ] as Map
}

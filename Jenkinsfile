@Library(['int-jenkins-shared@INT-6671_Tracking_Upstream_Dependencies']) _

pipeline {
  agent any
  stages {
    stage("Copy artifacts") {
      steps {
        script {
          if (currentBuild.previousSuccessfulBuild) {
             copyArtifacts(
                 projectName: currentBuild.projectName,
                 selector: specific("${currentBuild.previousSuccessfulBuild.number}")
             )
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
          createJiraIssue(            
            credentialId: 'jiraCredentialsLocal', 
            payload: createPayload(
              projectKey: 'TP', issueType: 'Task', 
              summary: 'summary title', reporter: 'admin'
              description: createDescription('Jenkins', latestVersion)
            ),
            baseUrl: 'http://localhost:2990/jira'
          )
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

String createPayload(Map<String, String> args = [:]) {
  return """{ 
  "fields": {
      "project": {
         "key": "${args.projectKey}"
      },
      "summary": "${args.summary}",
      "description": "${args.description}",
      "issuetype": {
         "name": "${args.issueType}"
      },
      "reporter": "${args.reporter}"
  }
}"""
}

String createDescription(String product, String version) {
  return """
h4. AC
 * The ${product} plugin works with ${product} ${version}
 * The ${product} plugin wdocumentation is updated accordingly
"""
}

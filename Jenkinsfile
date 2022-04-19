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
          if (latestVersion) {
            echo "Newer available version found: ${latestVersion}"
            String payload = createPayload(
                projectKey: 'TP', issueType: 'Task', reporter: 'admin',
                summary: 'summary title',
                description: createDescription('Jenkins', latestVersion)
            )            
            createJiraIssue(            
              credentialId: 'jiraCredentialsLocal', 
              payload: payload,
              baseUrl: 'http://localhost:2990/jira'
            )
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

String createPayload(Map<String, String> args = [:]) {
  return """{ 
  "fields": {
      "project": {
          "key": "${args.projectKey}"
      },
      "issuetype": {
          "name": "${args.issueType}"
      },
      "reter": {
          "name": "${args.reporter}"
      },
      "summary": "${args.summary}",
      "description": "${args.description}"      
  }
}"""
}

String createDescription(String product, String version) {
  return "h4. AC\\n" +
    " * The ${product} plugin works with ${product} ${version}\\n" +
    " * The ${product} plugin documentation is updated accordingly"
}

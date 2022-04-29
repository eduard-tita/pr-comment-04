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
    stage('Check versions') {
      parallel {
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
        stage("Gitlab") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'gitlab/lastVersion.txt',
                  url: 'https://gitlab.com/gitlab-org/gitlab/-/raw/master/CHANGELOG.md',
                  pattern: '\\n## ([0-9\\.]+) \\('
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Gitlab plugin', 'Gitlab', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Bamboo") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'bamboo/lastVersion.txt',
                  url: 'https://my.atlassian.com/download/feeds/bamboo.rss',
                  pattern: '<title>([0-9\\.]+) - ZIP Archive</title>'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Bamboo plugin', 'Bamboo', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Jira") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'jira/lastVersion.txt',
                  url: 'https://my.atlassian.com/download/feeds/jira-software.rss',
                  pattern: '<title>([0-9\\.]+) \\(ZIP Archive\\)</title>'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Jira plugin', 'Jira Server', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Maven") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'maven/lastVersion.txt',
                  url: 'https://github.com/apache/maven/tags.atom',
                  pattern: '<link rel="alternate" type="text/html" href="https://github.com/apache/maven/releases/tag/maven-([0-9\\.]+)"/>'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Maven plugin', 'Maven', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("IDEA") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'idea/lastVersion.txt',
                  url: 'https://data.services.jetbrains.com/products/releases?code=IIU&latest',
                  pattern: '"version":"([0-9\\.]+)"'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('IDEA plugin', 'IDEA', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Eclipse") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'eclipse/lastVersion.txt',
                  url: 'https://projects.eclipse.org/projects/eclipse/governance',
                  pattern: '<a href="/projects/eclipse/releases/[^"]+">([0-9\\.]+)</a>'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Eclipse plugin', 'Eclipse', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Visual Studio") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'visualStudio/lastVersion.txt',
                  url: 'https://docs.microsoft.com/en-us/visualstudio/releases/2022/release-notes',
                  pattern: '<h2 [^>]+>.+?Visual Studio 2022 version ([0-9\\.]+)'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Visual Studio plugin', 'Visual Studio 2022', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }
        stage("Operator SDK") {
          steps {
            script {
              String latestVersion = checkUpstreamVersion versionFile: 'operatorSdk/lastVersion.txt',
                  url: 'https://github.com/operator-framework/operator-sdk/releases.atom',
                  pattern: '<link rel="alternate" type="text/html" href="https://github.com/operator-framework/operator-sdk/releases/tag/v([0-9\\.]+)"/>'
              if (latestVersion) {
                echo "Newer available version found: ${latestVersion}"
                Map payload = createPayload('Sonatype Operators', 'Operator SDK', latestVersion)
                def newIssue = jiraNewIssue issue: payload, site: 'Local Jira'
                echo "New Jira issue created: ${newIssue.data.key}"
              }
            }
          }
        }



      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: '**/*.txt'
    }
  }
}

static Map createPayload(String plugin, String product, String version) {
  return [
      fields: [
          project: [key: 'TP'],
          issuetype: [name: 'Task'],
          summary: "Check ${plugin} compatibility with ${product} version ${version}",
          description: """
h4. AC
 * The ${plugin} works with ${product} version ${version}
 * The ${plugin} documentation is updated accordingly
"""
      ]
  ] as Map
}

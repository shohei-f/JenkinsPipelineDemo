pipeline {
    agent any

    stages {
        stage('Jira') {
            steps {
                script {
                    withEnv(['JIRA_SITE=LOCAL']) {
                        def csvFilePath = '/var/jenkins_home/workspace/JiraTest/sample.csv'
                        def csvData = readFile csvFilePath
                        def lines = csvData.readLines()

                        if (lines.size() > 1) {
                            def epicKey = ''
                            lines.drop(1).eachWithIndex { line, lineNum ->
                                def columns = line.split(',')
                                def issueType = columns[3].trim()
                                
                                if (issueType == 'エピック') {
                                    def epicIssue = createJiraIssue(columns, issueType)
                                    def response = jiraNewIssue issue: epicIssue
                                    epicKey = response.data.key
                                } else if (issueType == 'タスク') {
                                    def taskIssue = createJiraIssue(columns, issueType, epicKey)
                                    def response = jiraNewIssue issue: taskIssue
                                }
                            }
                        } else {
                            echo "CSV file has less than 2 lines."
                        }
                    }
                }
            }
        }
    }
}

def createJiraIssue(columns, issueType, parentKey = '') {
    def projectKey = columns[0].trim()
    def summary = columns[1].trim()
    def description = columns[2].trim()

    def issueFields = [
        project: [key: projectKey],
        summary: summary,
        description: description,
        issuetype: [name: issueType]
    ]
    
    if (issueType == 'タスク') {
        issueFields.parent = [key: parentKey]
    }
    
    return [fields: issueFields]
}

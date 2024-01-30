pipeline {
    agent any
    stages {
        stage('jira') {
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
                                if (issueType == 'エピック') {
                                    def epicIssue = [fields: [
                                        project: [key: columns[0].trim()],
                                        summary: columns[1].trim(),
                                        description: columns[2].trim(),
                                        issuetype: [name: columns[3].trim()]
                                    ]]
                                    response = jiraNewIssue issue: epicIssue    
                                    echo response.successful.toString()
                                    echo response.data.toString()
                                    
                                    // Call JIRA REST API to create Epic
                                    epicKey = response.data.key                                
                                } else if (issueType == 'タスク') {
                                    def columns = line.split(',')
                                    def taskIssue = [fields: [
                                        project: [key: columns[0].trim()],
                                        summary: columns[1].trim(),
                                        description: columns[2].trim(),
                                        issuetype: [name: columns[3].trim()],
                                        parent: [key: epicKey]
                                    ]]
                                    response = jiraNewIssue issue: taskIssue    
                                    echo response.successful.toString()
                                    echo response.data.toString()
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

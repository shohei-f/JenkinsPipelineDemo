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
                            lines.drop(1).eachWithIndex { line, lineNum ->
                            echo line
                            def columns = line.split(',')
                            def jiraIssue = [fields: [
                                project: [key: columns[0]],
                                summary: columns[1],
                                description: columns[2],
                                issuetype: [name: columns[3]]
                            ]]
                            response = jiraNewIssue issue: jiraIssue    
                            echo response.successful.toString()
                            echo response.data.toString()
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

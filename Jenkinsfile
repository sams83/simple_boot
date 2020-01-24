pipeline {
    agent any
    
    tools {
        maven 'M3'
    }
    
    stages {
        stage ('Checkout') {
            steps {
                sh 'echo "--=-- Checkout stage --=--"'
                git url:'https://github.com/Formation67/simple_boot.git'
            }
        }
        stage ('Compile') {
            steps {
                sh 'echo "--=-- Compile Stage --=--"'
                sh 'mvn clean compile'
            }
        }
        stage ('Test') {
            steps {
                sh 'echo "--=-- Test Stage --=--"'
                sh 'mvn test'
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
        stage ('Code coverage') {
            steps {
                jacoco (
                    execPattern: 'target/*.exec',
                    classPattern: 'target/classes',
                    sourcePattern: 'src/main/java',
                    exclusionPattern: 'src/test*'
                )
            }
        }
        stage ('Sanity check') {
            steps {
                sh 'echo "--=-- Sanity check test projet --=--"'
                sh 'mvn checkstyle:checkstyle pmd:pmd'
            }
            post {
                always {
                    recordIssues enabledForFailure: true, tools: [checkStyle()]
                    recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
                }
            }
        }
    }
}
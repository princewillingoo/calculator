pipeline {
    agent any
    tools{
        jdk "JDK17"
        gradle "Gradle8"
    }
    
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
                }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
        stage("Code coverage") {
            steps {
                sh "./gradlew jacocoTestReport"
                publishHTML (target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: "JaCoCo Report"
                    ])
                sh "./gradlew jacocoTestCoverageVerification"
                sh "uptime"
            }
        }
        stage("Static code analysis") {
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML (target: [
                    reportDir: 'build/reports/checkstyle/',
                    reportFiles: 'main.html',
                    reportName: "Checkstyle Report"])
            }
        }
        stage("Package") {
            steps {
                sh "./gradlew build"
            }
        }
        stage("Docker build") {
            steps {
                sh "pwd"
                sh "cat ~/cky.txt | docker login --username princewillingoo --password-stdin"
                sh "docker build -t princewillingoo/calculator ."
            }
        }
        stage("Docker Push"){
            steps {
                sh "docker push princewillingoo/calculator"
            }
        }
        stage("Deploy to staging") {
            steps {
                sh "docker run -d --rm -p 8765:8080 --name calculator
                leszko/calculator"
            }
        }
    }
}


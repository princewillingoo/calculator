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
                sh "docker build -t princewillingoo/calculator ."
            }
        }

        stage("Deploy to staging") {
            steps {
                sh "docker run -d --rm -p 8765:8080 --name calculator princewillingoo/calculator"
            }
        }

        stage("Acceptance test") {
            steps {
                sleep 60
                sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }
        }
    }
    
    post {
        always {
            sh "docker stop calculator"
        }
    }
}


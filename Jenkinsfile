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
    }
}
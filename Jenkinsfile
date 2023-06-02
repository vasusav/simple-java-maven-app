pipeline {
    agent {
         any{
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('build && SonarQube analysis') {
            steps {
                // sh 'mvn test'
                // withSonarQubeEnv("${SONARSERVER}") {
                withSonarQubeEnv('My SonarQube Server', envOnly: true) {
                    // Optionally use a Maven environment you've configured already
                    // withMaven(maven:'Maven 3.5') {
                    sh 'mvn clean package sonar:sonar'
                    }
                // }
            }
        }
    }
}

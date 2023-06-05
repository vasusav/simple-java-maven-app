pipeline {
    agent {
         any{
            image 'maven:3.9.0'
            args '-v /root/.m2:/root/.m2'
        }
    }
    environment{
        SONARSERVER = 'sonarserver'
    }
    stages {
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                    sh 'mvn clean package sonar:sonar'
                    }
            }
        }
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
        stage('Transfer to Artifactory') {
            steps {
                script {
                    sh 'mv ~/.jenkins/workspace/fp-java-maven/target/my-app-1.0-SNAPSHOT.jar ~/.jenkins/workspace/fp-java-maven/target/root.war'
                    sh 'jfrog rt u ~/.jenkins/workspace/fp-java-maven/target/root.war fp-java-maven/root.war'
                }
            }
        }
        stage('Deploy to tomcat server') {
            steps {
                script {
                    sh 'mv ~/.jenkins/workspace/fp-java-maven/target/root.war /usr/local/Cellar/tomcat/10.1.9/libexec/webapps/root.war'
                }
            }
        }
    }
}

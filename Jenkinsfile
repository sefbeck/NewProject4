pipeline {
    agent any

    stages {
        stage('Gitcheckout') {
            steps {
                git 'https://github.com/sefbeck/NewProject4.git'
            }
        }
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn package'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
               deploy adapters: [tomcat9(credentialsId: 'devops', 
               path: '', 
               url: 'http://34.230.5.22:8080/')], 
               contextPath: 'default',
               war: '**/*.war'
            }
        }
    }
}

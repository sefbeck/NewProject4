pipeline {
    agent any

    stages {
        stage('Gitcheckout') {
            steps {
                git 'https://github.com/sefbeck/NewProject4.git'
            }
        }
       
        stage('SonarQube analysis') {
            steps {
              withSonarQubeEnv( 'sonar') {
                sh 'mvn -f SampleWebApp/pom.xml clean package sonar:sonar'
              }
            }
         }
     
        
        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar-cred'
            }
        }
        stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package -Dbuild.number=${BUILD_NUMBER}'
            }
        }
        
        stage('Deploy to jfrog') {
            steps {
                rtUpload (
                    serverId: 'my-jfrog',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "**/*.war",
                            "target": "my-repo/"
                            }
                        ]
                    }''',
                )
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

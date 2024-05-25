pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar_server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
               nexusArtifactUploader artifacts: [[artifactId: 'Samplewebapp', classifier: '', file: 'samplewebapp/targets/samplewebapp.war', type: 'war']], credentialsId: 'maven-snapshots', groupId: 'Samplewebapp', nexusUrl: 'ec2-54-91-138-139.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-snapshot'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
            deploy adapters: [tomcat9(credentialsId: 'Tomcat-server', path: '', url: 'http://34.229.13.91:8080')], contextPath: 'webapp', war: '**/*.war'
              
              
              
          }
            
        }
            
        }
} 

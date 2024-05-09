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
                nexusArtifactUploader artifacts: [[artifactId: 'samplewebapp', classifier: '', file: 'samplewebApp/targets/samplewebapp.war', type: 'war']], credentialsId: '', groupId: 'samplewebapp', nexusUrl: 'ec2-100-27-217-139.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0_snapshots'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
             deploy adapters: [tomcat9(path: '', url: 'http://54.197.31.215:8080')], contextPath: 'webapp', war: '**/*war
              
              
          }
            
        }
            
        }
} 

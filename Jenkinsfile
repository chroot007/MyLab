
pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
   
    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        //Stage 3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'OssyDevOpsLab', classifier: '', file: 'target/OssyDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '8afd332d-c6ab-4e70-a551-b6c5305ea817', groupId: 'com.ossydevopsLab', nexusUrl: '172.20.10.200:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'OssyDevOpsLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }
        // Stage4 : Deploying
          stage ('Deploy'){
            steps {
                echo ' deploying......'

            }
        }
    }
}
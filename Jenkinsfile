
pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
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
              script {
            
              def NexusRepo = Version.endsWith("SNAPSHOT") 
              ? "OssyDevOpsLab-SNAPSHOT" : "OssyDevOpsLab-RELEASE"


              nexusArtifactUploader artifacts:
               [[artifactId: "${ArtifactId}", 
               classifier: '', 
               file: "target/${ArtifactId}-${Version}.war", 
               type: 'war']], 
               credentialsId: '3bc95edd-f40a-4cd4-bd41-c1e83da38b62',
                groupId: "${GroupId}", 
                nexusUrl: '172.20.10.121:8081',
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: "${NexusRepo}", 
                 version: "${Version}"
                 }
            }
        }
    
        // Stage 4 : Deploying
          stage ('Deploy'){
            steps {
                echo ' deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller',
                 transfers: 
                 [sshTransfer(cleanRemote: false, 
                 excludes: '', 
                 execCommand: 'ansible-playbook /opt/playbooks/downloadDeploy.yaml -i /opt/playbooks/hosts', 
                 execTimeout: 120000, 
                 )],
                     usePromotionTimestamp: false,
                      useWorkspaceInPromotion: false, 
                      verbose: false)])

            }
        }

         // Stage 5 : Deploying the build artifact to docker
         stage ('Deploy'){
            steps {
                echo ' deploying......'
                sshPublisher(publishers: 
                [sshPublisherDesc(configName: 'Ansible_Controller',
                 transfers: 
                 [sshTransfer(cleanRemote: false, 
                 excludes: '', 
                 execCommand: 'ansible-playbook /opt/playbooks/downloadDeploy_docker.yaml -i /opt/playbooks/hosts', 
                 execTimeout: 120000, 
                 )],
                     usePromotionTimestamp: false,
                      useWorkspaceInPromotion: false, 
                      verbose: false)])

            }
        }
    }
}
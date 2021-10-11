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
               echo 'Testing...'  

            }
        }
        
        //Stage2 : Publish the artifact to Nexus
        stage ('Publish to Nexus'){
            steps {
                script{

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"
                
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: "target/${ArtifactId}-${Version}.war",
                type: 'war']], 
                credentialsId: 'c91be929-e5ab-4a32-929e-b11f068ed25c', 
                groupId: "${GroupId}",
                nexusUrl: '192.168.2.120:8081',
                nexusVersion: 'nexus3',
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
               }
           }
        }
        
         // Stage 4 : Print some information
        stage ('Print Environment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
        }
        
        
        // Stage5 : Publish the source code to Sonarqube
        stage ('Deploy'){
            steps {
                echo ' Deploying......'
                
             }

        }
   
        
    }

}

pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "3.83.158.224:8081"
        NEXUS_REPOSITORY = "maven-central-repo"
        NEXUS_CREDENTIAL_ID = "NEXUS_CRED"
    }
   stages {
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: '3.83.158.224:8081',
                            groupId: 'pom.in.javahome',
                            version: 'pom.1.0-SNAPSHOT',
                            repository: 'maven-central-repo',
                            credentialsId: 'NEXUS_ID',
                            artifacts: [
                                [artifactId: 'pom.car-rentals',
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.car-rentals,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}

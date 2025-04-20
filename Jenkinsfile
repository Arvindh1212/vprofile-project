pipeline {
    agent any

    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-centrl'           // ✅ correct value from Nexus
        NEXUSIP = '172.31.16.97'                      // ✅ your internal Nexus IP
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'           // ✅ assuming group repo name
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
    }

    stages {
        stage('Prepare settings.xml') {
            steps {
                echo '[INFO] Replacing environment variables in settings-template.xml'
                sh '''
                    envsubst < settings-template.xml > settings.xml
                    echo "Generated settings.xml:"
                    cat settings.xml
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }

        }

        stage('Checkstyle Analysis'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
    }
}


        

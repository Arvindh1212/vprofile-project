pipeline {
    agent any

    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-centrl'
        NEXUSIP = '172.31.16.97'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin123'
    }

    stages {
        stage('Prepare settings.xml') {
            steps {
                script {
                    sh '''
                    echo "[INFO] Replacing environment variables in settings-template.xml"
                    envsubst < settings-template.xml > settings.xml
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}

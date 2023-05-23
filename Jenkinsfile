pipeline {
    agent any
    tools {
        maven "maven3"
        jdk "jdk8"
    }

    environment {
        SNAP_REPO = 'zack2-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = '123'
        RELEASE_REPO = 'zack2-release'
        CENTRAL_REPO = 'zack2-maven-central'
        NEXUSIP = '172.31.31.156'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'zack2-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }



    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }

            post {
                success {
                    echo "now Archiving.."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }

        }
    }
}
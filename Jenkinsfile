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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
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

        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis'){
            steps{
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }

        stage('Sonar Analisys') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
    }
}
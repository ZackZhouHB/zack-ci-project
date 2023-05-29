def buildNumber = Jenkins.instance.getItem('zack2-cicdbean-stage').lastSuccessfulBuild.number


def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]

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
        NEXUSIP = '172.31.21.172'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'zack2-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
        ARTIFACT_NAME = "vprofile-v${buildNumber}.war"
        AWS_S3_BUCKET = 'zack2-cicd-bean'
        AWS_EB_APP_NAME = 'zack2-cicd-bean-prod'
        AWS_EB_ENVIRONMENT = 'Zack2-cicd-bean-prod-env'
        AWS_EB_APP_VERSION ="${buildNumber}"
    }



    stages {
        stage('Deploy to Bean Prod') {
          steps {          
            withAWS(credentials: 'awsbeancreds', region: 'ap-southeast-2') {
              sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT --version-label $AWS_EB_APP_VERSION'
            }
          }
        }
    }
    post {
        always {
            echo 'Slack Notification.'
            slackSend channel: '#cidi',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
}
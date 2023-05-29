def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]

pipeline {
    agent any
    environment {
        NEXUS_PASS = credentials('nexuspass')
    }

    stages {

        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            string(
                                defaultValue: '',
                                name: 'BUILD',
                            ),
                            string(
                                defaultValue: '',
                                name: 'BUILD',
                            )
                        ])
                    ])
                }

            }
        }
            

        stage('Ansible Deploy to Prod'){
            steps {
                ansiblePlaybook([
                inventory   : 'ansible/prod.inventory',
                playbook    : 'ansible/site.yml',
                installation: 'ansible',
                colorized   : true,
			    credentialsId: 'applogin-prod',
			    disableHostKeyChecking: true,
                extraVars   : [
                   	USER: "admin",
                    PASS: "${NEXUS_PASS}",
			        nexusip: "172.31.21.172",
			        reponame: "zack2-release",
			        groupid: "QA",
			        time: "${env.TIME}",
			        build: "${env.BUILD}",
                    artifactid: "vproapp",
			        vprofile_version: "vproapp-${env.BUILD}-${env.TIME}.war"
                ]
             ])
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
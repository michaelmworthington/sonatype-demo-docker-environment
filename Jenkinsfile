IMAGE_NAME="host.docker.internal:19447/michaelmworthington/fluentd-influxdb:v1.7.4-debian-2.0--68"
CONTAINER_NAME="fluentd-influxdb"
IQ_NAME="fluentd-influxdb"

pipeline {
    agent any
    stages {
        stage('SCM'){
            steps {
                checkout scm: filesystem(clearWorkspace: true, path: '/Users/mworthington/projects/GIT-Projects/gitlab.com/michaelmworthington/sonatype-demo-environment/docker-compose')
            }
        }
        stage('Build'){
            steps {

                //TODO: Set an environment variable for the build number to use in the image tag

                //pull the new latest
                //sh "docker-compose -p docker-compose pull --quiet " + CONTAINER_NAME
                //stop and remove the current container
                sh 'docker-compose -p docker-compose stop --timeout 120 ' + CONTAINER_NAME + ' || /usr/bin/true'
                sh 'docker-compose -p docker-compose rm --force ' + CONTAINER_NAME + ' || /usr/bin/true'
                //start the new container
                sh 'docker-compose -p docker-compose up -d --build' + CONTAINER_NAME
                //push new image
                sh 'docker-compose -p docker-compose push' + CONTAINER_NAME
            }
        }

        stage('Policy Evaluation'){
            steps {
                //download the image and do a scan in IQ
                sh 'docker save -o ' + CONTAINER_NAME + '.tar ' + IMAGE_NAME

                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: IQ_NAME, iqScanPatterns: [[scanPattern: CONTAINER_NAME + '.tar']], iqStage: 'build', jobCredentialsId: ''
            }
        }
    }
}

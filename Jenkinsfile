pipeline {
    agent any
    stages {
        stage('Build'){
            steps {
                sh 'echo "hello world"'
            }
        }
        stage('Policy Evaluation'){
            steps {
                nexusPolicyEvaluation failBuildOnNetworkError: false, iqApplication: 'SonatypeDemoDockerEnvironment-JenkinsPipelineTest', iqScanPatterns: [[scanPattern: '**/*']], iqStage: 'build', jobCredentialsId: ''
            }
        }
    }
}

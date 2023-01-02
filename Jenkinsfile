node(){

    def sonarScanner = tool name: 'newtoken', type: 'hudson.plugins.sonar.SonarRunnerInstallation'

 

    
    try {
        stage('Checkout Code'){
           checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '4bcad23c-c719-448f-b24b-b85a66a02a35', url: 'https://github.com/updhanu/SonarQubeNodeJS']])
        }

        stage('NPM Build'){
            sh "npm clean install"
        }

        stage('Test Cases Execution'){
            echo "test successful"
        }

        stage('SonarQube Analysis'){
            withSonarQubeEnv(credentialsId: '5b27e7f4-248f-4da0-975d-6717cae07ec9') {
                sh "${sonarScanner}/bin/sonar-scanner"

}
        }


    }
    catch (Exception e){
        currentBuild.result = 'FAILURE'
        echo currentBuild.currentResult
    }finally{
        emailext attachLog: true, attachmentsPattern: 'target/surefire-reports/*.xml', 
             body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
    Check console output at $BUILD_URL to view the results.''', 
            compressLog: true, recipientProviders: [buildUser(), requestor()], subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!', to: 'sharanyaj295@gmail.com'
    }
}

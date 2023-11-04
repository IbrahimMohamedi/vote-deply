node {
    def app
    
    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github-access-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh "git config user.email ibrahim.a.eltigani@outlook.com"
                        sh "git config user.name IbrahimMohamedi"
                        sh "cat vote-ui-deployment.yaml"
                        sh "sed -i 's+ibrahimmohamedi/vote.*+ibrahimmohamedi/vote:${DOCKERTAG}+g' vote-ui-deployment.yaml"
                        sh "cat vote-ui-deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job deployment: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/vote-deply.git HEAD:main"
      }
    }
  }
}
     stage('Trigger deployment') {
      agent any
      environment{
        def GIT_COMMIT = "${env.GIT_COMMIT}"
      }
      steps{
        echo "${GIT_COMMIT}"
        echo "${GIT_COMMIT}"
        echo "triggering deployment"
        // passing variables to job deployment run by vote-deploy repository Jenkinsfile
        build job: 'deployment', parameters: [string(name: 'DOCKERTAG', value: GIT_COMMIT)]
      }    
   }
}

node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                bat "echo ${BRANCH}"
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        bat "git config user.email timwasl4@gmail.com"
                        bat "git config user.name Tim-Wasl"
                        //sh "git switch master"
                        if ( "${BRANCH}" == 'main') {
                            bat "type deployment.yaml"
                            test = readFile "deployment.yaml"
                            newconfig = test.replaceAll("timcicd/repository.*","timcicd/repository:${DOCKERTAG}")
                            writeFile file: "deployment.yaml", text: "${newconfig}"
                            bat "type deployment.yaml"
                            bat "git add ."
                            bat """git commit -m \"Done by Jenkins Job changemanifest: ${DOCKERTAG} \""""
                            bat "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                        }
                        else if ("${BRANCH}" == 'develop') {
                             bat "type .\\test\\deployment.yaml"
                            test = readFile ".\\test\\deployment.yaml"
                            newconfig = test.replaceAll("timcicd/testrepository.*","timcicd/testrepository:${DOCKERTAG}")
                            writeFile file: ".\\test\\deployment.yaml", text: "${newconfig}"
                            bat "type .\\test\\deployment.yaml"
                            bat "git add ."
                            bat """git commit -m \"Done by Jenkins Job test changemanifest: ${DOCKERTAG} \""""
                            bat "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
                         }
      }
    }
  }
}
}

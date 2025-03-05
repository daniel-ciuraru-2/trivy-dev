// pipeline {
//     agent any

//     stages {
//         stage('Checkout') {
//             steps {
//                 git branch: 'main', url: "${REPO_URL}"
//             }
//         }

//         stage('Run Trivy Scan') {
//             steps {
//                 script {
//                     withCredentials([
//                         string(credentialsId: 'AQUA_KEY_22263', variable: 'AQUA_KEY'),
//                         string(credentialsId: 'AQUA_SECRET_22263', variable: 'AQUA_SECRET')
//                     ]) {
//                         script {
//                             sh '''
//                             printenv
//                             export AQUA_KEY=$AQUA_KEY
//                             export AQUA_SECRET=$AQUA_SECRET
//                             export TRIVY_RUN_AS_PLUGIN=aqua
//                             export CSPM_URL=https://stage.api.cloudsploit.com
//                             export AQUA_URL=https://api.dev.supply-chain.cloud.aquasec.com
//                             docker run --rm \
//                                 -v $(pwd):/repo \
//                                 -e AQUA_KEY=$AQUA_KEY \
//                                 -e AQUA_SECRET=$AQUA_SECRET \
//                                 -e TRIVY_RUN_AS_PLUGIN=aqua \
//                                 -e CSPM_URL=$CSPM_URL \
//                                 -e AQUA_URL=$AQUA_URL \
//                                 ${TRIVY_IMAGE} trivy fs --debug --scanners vuln --list-all-pkgs /repo
//                             '''
//                         }
//                     }
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/daniel-ciuraru-2/performance-test.git" // Change this
    }

  stages {
    stage('Checkout') {
        steps {
            git branch: 'main', url: "${REPO_URL}"
        }
    }
    stage('Aqua scanner') {
      agent {
        docker {
          image 'aquasec/aqua-scanner:dev'
        }
      }
      steps {
        withCredentials([
          string(credentialsId: 'AQUA_KEY_22263', variable: 'AQUA_KEY'),
          string(credentialsId: 'AQUA_SECRET_22263', variable: 'AQUA_SECRET'),
          string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')
        ]){
          sh '''
            export TRIVY_RUN_AS_PLUGIN=aqua
            export AQUA_URL=https://api.dev.supply-chain.cloud.aquasec.com
            export CSPM_URL=https://stage.api.cloudsploit.com
            trivy fs --debug --scanners vuln --list-all-pkgs .
          '''
        }
      }
    }
  }
}

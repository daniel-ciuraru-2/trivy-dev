pipeline {
    agent any

    environment {
        REPO_URL = "https://github.com/daniel-ciuraru-2/performance-test.git" // Change this if needed
    }

    stages {
        stage('Aqua scanner') {
            agent {
                docker {
                    image 'daniel86c/pubtest:trivylocal'
                    alwaysPull true
                }
            }
            steps {
                git branch: 'main', url: env.REPO_URL
                
                withCredentials([
                    string(credentialsId: 'AQUA_KEY_22263', variable: 'AQUA_KEY'),
                    string(credentialsId: 'AQUA_SECRET_22263', variable: 'AQUA_SECRET'),
                    string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')
                ]) {
                    script {
                        sh '''
                            printenv
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
}
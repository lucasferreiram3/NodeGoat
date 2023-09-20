pipeline {
    agent any 
    environment {
        caminhoPacote = 'uploadToVeracode/nodegoat.tar.gz'
        wrapperVersion = '23.8.12.0'
    }
    stages {
        stage('Clean') { 
            steps {
                sh 'rm -rf pipeline-scan-LATEST.zip pipeline-scan.jar'
                sh 'rm -rf veracode-wrapper.jar'
                sh 'rm -rf uploadToVeracode'
            }
        }
        stage('Archive') { 
            steps {
                sh 'mkdir uploadToVeracode'
                sh 'find . -name "*.js" -o -name "*.html" -o -name "*.htm" -o -name "*.ts" -o -name "*.tsx" -o -name "*.json" -o -name "*.css" -o -name "*.jsp" -o -name "*.vue" | tar --exclude=./uploadToVeracode --exclude=./.git --exclude=./.gihtub -cvzf uploadToVeracode/nodegoat.tar.gz .'
            }
        }

        stage('Veracode SAST - Pipeline Scan') { 
            steps {
                withCredentials([usernamePassword(credentialsId: 'veracode-credentials', passwordVariable: 'VKEY', usernameVariable: 'VID')]) {
                sh 'curl -sSO https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip'
                sh 'unzip -o pipeline-scan-LATEST.zip'
                sh (""" java -jar pipeline-scan.jar \
                    --veracode_api_id "${VID}" \
                    --veracode_api_key "${VKEY}" \
                    --file ${caminhoPacote} \
                    --project_name "NodeGoat-Demo" \
                    """)
                }
            }
        }
    }
}
pipeline { 
    agent { label 'docker-agent' } 

    stages { 
        stage('Clone Repository') { 
            steps { 
                sh 'rm -rf LocalFlaskAppBuild || true' 
                sh 'git clone https://github.com/Sofiya-19/LocalFlaskAppBuild.git' 
                echo "Repository cloned successfully." 
            } 
        } 

        stage('Stop Previous Flask Instance') {
            steps {
                sh 'pkill -f app.py || true'  
                echo "Stopped any running Flask instances to avoid port conflicts."
            }
        }

        stage('Run Flask App') { 
            steps { 
                dir('LocalFlaskAppBuild') { 
                    sh '''
                    python3 app.py > flask.log 2>&1 &
                    ''' 
                    sh 'tail -f flask.log' 
                } 
            } 
        } 
    } 

    post { 
        always { 
            emailext ( 
                to: 'sofiyabg.aids2021@citchennai.net', 
                subject: "Jenkins Build Status: ${currentBuild.currentResult}", 
                body: """Build Summary:
                - Job Name: ${env.JOB_NAME} 
                - Build Number: ${env.BUILD_NUMBER} 
                - Build Status: ${currentBuild.currentResult} 
                - Build URL: ${env.BUILD_URL} 
                """, 
                mimeType: 'text/plain' 
            ) 
        } 
    } 
}

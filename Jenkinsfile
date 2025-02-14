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

        stage('Run') { 
            steps { 
                dir('LocalFlaskAppBuild') { 
                    sh 'python3 app.py > flask.log 2>&1 &' 
                    sh 'tail -f flask.log' 
                } 
            } 
        } 
    } 

    post { 
        always { 
            emailext ( 
                to: 'sofiyabalamurugan@gmail.com', 
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

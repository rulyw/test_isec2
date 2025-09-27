pipeline {
    agent any

    stages {
        stage('Test Docker Access') {
            steps {
                script {
                    echo "Checking Docker access..."
                    def result = sh(script: 'docker version', returnStatus: true)
                    
                    if (result == 0) {
                        echo "✅ Jenkins has Docker access!"
                    } else {
                        error "❌ Jenkins cannot access Docker! Check installation or permissions."
                    }
                }
            }
        }
    }
}

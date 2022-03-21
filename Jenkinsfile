pipeline {
    agent { node { label 'maven' } }
        stages {
            stage('Test') {
                steps {
                    sh 'chmod +x mvnw'
                    sh './mvnw clean test'
            }
        }
    stage('Build') {
        environment {
            QUAY = credentials('MONITORING_LAB_QUAY_CREDENTIALS')
        }
        steps {
            sh 'chmod +x ./scripts/include-container-extensions.sh'
            sh './scripts/include-container-extensions.sh'
            sh 'chmod +x ./scripts/build-and-push-image.sh'
            sh '''
                ./scripts/build-and-push-image.sh \
                -b $BUILD_NUMBER \
                -u "$QUAY_USR" \
                -p "$QUAY_PSW" \
                -r do400-monitoring-lab
            '''
            }
        }
    stage('Deploy') {
        steps {
            sh 'chmod +x ./scripts/redeploy.sh'
            sh '''
                ./scripts/redeploy.sh \
                -d calculator \
                -n thason-monitoring-lab
            '''
}
}   
    }
    post {
        failure {
            mail to: "team@example.com",
                subject: "Pipeline failed: ${currentBuild.fullDisplayName}",
                body: "The following pipeline failed: ${env.BUILD_URL}"
        }
    }
}
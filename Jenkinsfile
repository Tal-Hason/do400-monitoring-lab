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
            sh 'chmod +x /scripts/include-container-extensions.sh'
            sh './scripts/include-container-extensions.sh'
            sh 'chmod +x /scripts/build-and-push-image.sh'
            sh '''
                ./scripts/build-and-push-image.sh \
                -b $BUILD_NUMBER \
                -u "$QUAY_USR" \
                -p "$QUAY_PSW" \
                -r do400-monitoring-lab
            '''
            }
        }   
    }
}
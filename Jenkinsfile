
pipeline {
    agent any

    environment {
        APP_NAME = 'student-app'
        TOMCAT_WEBAPPS = '/opt/tomcat/webapps'
    }

    stages {

        stage('1. Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/tanay25/java-maven-test.git'
            }
        }

        stage('2. Check Java and Maven') {
            steps {
                sh '''
                    java -version
                    mvn -version
                '''
            }
        }

        stage('3. Build Application') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('4. Test Application') {
            steps {
                sh 'mvn test'
            }
        }

        stage('5. Create WAR') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/student-app.war',
                                 fingerprint: true
            }
        }

        stage('6. Deploy to Tomcat') {
            steps {
                sh '''
                    cp target/student-app.war \
                    ${TOMCAT_WEBAPPS}/student-app.war

                    sudo systemctl restart tomcat

                    sleep 10

                    test -f ${TOMCAT_WEBAPPS}/student-app.war
                '''
            }
        }
    }

    post {
        success {
            echo 'Java JSP application deployed successfully!'
        }

        failure {
            echo 'Jenkins pipeline failed. Check console output.'
        }
    }
}

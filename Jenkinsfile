pipeline {
    agent any

    environment {
        scannerHome = tool 'sonar8.0'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn -f pom.xml install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving it...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST') {
            steps {
                sh 'mvn -f pom.xml test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn -f pom.xml checkstyle:checkstyle'
            }
        }

        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonarqubeserver') {
                    sh '''
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=java-tomcat-sample \
                    -Dsonar.projectName=java-tomcat-sample \
                    -Dsonar.projectVersion=4.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                    '''
                }
            }
        }

        // stage('UploadArtifact') {
        //     steps {
        //         nexusArtifactUploader(
        //             nexusVersion: 'nexus3',
        //             protocol: 'http',
        //             nexusUrl: '172.31.25.23:8081',
        //             groupId: 'QA',
        //             version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}", //installing the timestamp in jenkins plugins
        //             repository: 'sonarqube_nexus',
        //             credentialsId: 'sonartypecred',
        //             artifacts: [
        //                 [
        //                     artifactId: 'java-tomcat-sample',
        //                     classifier: '', //default null
        //                     file: 'target/java-tomcat-maven-example.war',
        //                     type: 'war'
        //                 ]
        //             ]
        //         )
        //     }
        // }

    }
}

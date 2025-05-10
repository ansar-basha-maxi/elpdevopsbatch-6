pipeline {
    agent any

    tools {
        maven 'Maven-3.6.1'
    }

    stages {

        stage('Preparation') {
            steps {
                echo "Cloning the Git repository..."
                git 'https://github.com/raknas999/hello-world-servlet.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project using Maven..."
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        }

        stage('Results') {
            steps {
                echo "Publishing test results and archiving build artifacts..."
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        // Optional: SonarQube code quality analysis
        /*
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        */

        stage('Artifact Upload') {
            steps {
                echo "Uploading artifact to Nexus repository..."
                nexusArtifactUploader(
                    artifacts: [[
                        artifactId: 'hello

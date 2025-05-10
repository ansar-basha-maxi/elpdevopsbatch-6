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
                        artifactId: 'hello-world-servlet-example',
                        classifier: '',
                        file: 'target/helloworld.war',
                        type: 'war'
                    ]],
                    credentialsId: '0305',
                    groupId: 'com.geekcap.vmturbo',
                    nexusUrl: '3.144.167.101:8081/nexus',
                    nexusVersion: 'nexus2',
                    protocol: 'http',
                    repository: 'Releases',
                    version: "${BUILD_NUMBER}"
                )
            }
        }

        // Optional: Deployment to Tomcat via SCP or curl
        /*
        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying WAR to Tomcat server..."
                sshagent(['tomcat-creds-login']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/helloworld.war ec2-user@3.20.240.174:/opt/apache-tomcat-8.5.87/webapps
                    """
                }

                sh 'curl -T target/helloworld.war "http://admin:admin@3.20.240.174:8080/manager/text/deploy?path=/myproject&update=true"'
            }
        }
        */

        // Optional: Deployment using Ansible
        /*
        stage('Deploy with Ansible') {
            steps {
                echo "Deploying using Ansible..."
                sh 'ansible-playbook deploy.yml'
            }
        }
        */
    }

    post {
        success {
            mail to: 'arjunrayewar@gmail.com',
                 subject: "SUCCESS: ${currentBuild.fullDisplayName}",
                 body: "Build completed successfully."
        }
        failure {
            mail to: 'arjunrayewar@gmail.com',
                 subject: "FAILURE: ${currentBuild.fullDisplayName}",
                 body: "Build failed. Please check the Jenkins logs."
        }
    }
}

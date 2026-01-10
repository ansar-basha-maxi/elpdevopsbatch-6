pipeline {
    agent any 
    tools { 
        maven 'Maven'
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purposes

      // Get some code from a GitHub repository

      git 'https://github.com/raknas999/hello-world-servlet.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.war'
      }
 }
//  stage('Sonarqube') {
//     environment {
//         scannerHome = tool 'sonarqube'
//     }
//     steps {
//         withSonarQubeEnv('sonarqube') {
//             sh "${scannerHome}/bin/sonar-scanner"
//         }
//   //      timeout(time: 10, unit: 'MINUTES') {
//  //           waitForQualityGate abortPipeline: true
//   //      }
//     }
// }
     stage('Artifact upload') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'hello-world-servlet-example', classifier: '', file: '/var/lib/jenkins/workspace/New-Nexus/target/helloworld.war', type: 'war']], credentialsId: '0305', groupId: 'com.geekcap.vmturbo', nexusUrl: '3.144.167.101:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'Releases', version: '$BUILD_NUMBER'
    //  nexusPublisher nexusInstanceId: '0305', nexusRepositoryId: 'Sample', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/helloworld.war']], mavenCoordinate: [artifactId: 'hello-world-servlet-example', groupId: 'com.geekcap.vmturbo', packaging: 'war', version: '$BUILD_NUMBER']]]
      }
 }

//      stage('build code') {
//       steps { 
//         sshagent(['tomcat-creds-login'])
//         sh "scp -o StrictHostKeyChecking=no var/lib/jenkins/workspace/nexus-stage/target/helloworld.war ec2-user@3.20.240.174:/opt/apache-tomcat-8.5.87/webapps"
            
       
//       }
//  }
//      stage("deploy-dev"){
//        steps{
//           sh 'curl -T targetvar/lib/jenkins/workspace/nexus-stage/target/helloworld.war "http://admin:admin@3.20.240.174:8080/manager/text/deploy?path=/myproject&update=true" '
// }
        //   sshagent(['040120']) {
        //   sh """
        //   scp -o StrictHostKeyChecking=no target/var/lib/jenkins/workspace/New-Nexus/target/helloworld.war  
        //   ec2-user@ec2-3-20-240-174:/root/apache-tomcat-8.5.87/webapps
        //    """
        //     }
        //   }
        // }
//  stage('Artifact upload') {
//       steps {
//         sshagent(['040120']) {
//     // some block
// }
       
//       }
//  }
 
//      stage('Deploy War') {
//       steps {
//         sh label: '', script: 'ansible-playbook deploy.yml'
//       }
//  }
}
// post {
//         success {
//             mail to:"arjunrayewar@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Build success"
//         }
//         failure {
//             mail to:"arjunrayewar@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
//         }
//     }       
}


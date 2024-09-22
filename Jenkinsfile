pipeline {
  agent any

  tools{
    jdk 'jdk17'
    maven 'maven3'
  }

  stages {
    stage('Git Checkout'){
        steps{
            git branch: 'main', credentialsId: 'Github-Cred', url: 'https://github.com/B0o0H/Boardgame.git'
        }
    }
    stage('Complie'){
        steps{
            sh 'mvn complie'
        }
    }
    stage('Test'){
        steps{
            sh 'mvn test'
        }
    }
    // stage('Build'){
    //     steps{
    //         sh 'mvn package'
    //     }
    // }        
    stage('Sonar Scan'){
        steps{
            script{
                scannerHome = tool 'sonar-scanner'
            }
            withSonarQubeEnv('sonarqube') {
                sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Boardgame \
                    -Dsonar.projectName=Boardgame \
                    -Dsonar.java.binaries=.'''
            }        
        }
    }
    stage('Build Image'){
        steps{
            sh ''
        }
    } 
    stage('Push Image'){
        steps{
            sh ''
        }
    }
    stage('Deploy into ECS'){
        steps{
            sh ''
        }
    }   
//   stages {
//     stage('Test') {
//         agent {
//             ecs {
//                 inheritFrom 'label-of-my-preconfigured-template'
//                 cpu 2048
//                 memory 4096
//                 image '$AWS_ACCOUNT.dkr.ecr.$AWS_REGION.amazonaws.com/jenkins/java8:2019.7.29-1'
//                 logDriver 'fluentd'
//                 logDriverOptions([[name: 'foo', value:'bar'], [name: 'bar', value: 'foo']])
//                 portMappings([[containerPort: 22, hostPort: 22, protocol: 'tcp'], [containerPort: 443, hostPort: 443, protocol: 'tcp']])
//             }
//         }
//         steps {
//             sh 'echo hello'
//         }
//     }
//   }
    }
}
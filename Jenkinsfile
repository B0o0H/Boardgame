pipeline {
  agent any

  tools{
    jdk 'jdk17'
    maven 'maven3'
  }
  
  environment{
    region = "ap-southeast-2"
    cluster = "demoCluster"
    service = ""
    ecrRepo = "demo/boardgame"
  }

  stages {
    stage('Git Checkout'){
        steps{
            git branch: 'main', credentialsId: 'Github-Cred', url: 'https://github.com/B0o0H/Boardgame.git'
        }
    }
    stage('Complie'){
        steps{
            sh 'mvn compiler:compile'
        }
    }
    stage('Test'){
        steps{
            sh 'mvn test'
        }
    }      
    stage('Sonar Scan'){
        environment {
            scannerHome = tool 'sonar-scanner'
        }
        steps{
            withSonarQubeEnv('sonarqube') {
                sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Boardgame \
                    -Dsonar.projectName=Boardgame \
                    -Dsonar.java.binaries=.'''
            }        
        }
    }
    stage('Build'){
        steps{
            sh 'mvn package'
        }
    } 
    stage('Build $ Push Image to ECR'){
        steps{
            withAWS(credentials: 'AWS-Cred', region: 'ap-southeast-2'){
                script {
                    def identity = awsIdentity()
                    env.AWS_ACCOUNT = identity.account
                    sh '''aws ecr get-login-password --region ${region} | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.${region}.amazonaws.com \
                        docker build -t demo/boardgame . \
                        docker tag demo/boardgame:latest ${AWS_ACCOUNT}.dkr.ecr.${region}.amazonaws.com/${ecrRepo}:latest \
                        docker push ${AWS_ACCOUNT}.dkr.ecr.${region}.amazonaws.com/${ecrRepo}:latest'''
                }
            }
        }
    }
    stage('Deploy into ECS'){
        steps{
            withAWS(credentials: 'AWS-Cred', region: 'ap-southeast-2'){
                // sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
                sh ''' echo "pass" '''
            }
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
pipeline {
    
    agent {
        label "build"
    }
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/kenchedda/CICD-EKS-NEXUS.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                    }
                }
            }
            stage('nexus_uploader') {
                steps{
                    script {
                        nexusArtifactUploader artifacts:
                         [
                            [
                                artifactId: 'springboot',
                                classifier: '',
                                file: 'target/Uber.jar',
                                type: 'jar'
                                ]
                            ], 
                                credentialsId: 'nexus_cred', 
                                groupId: 'com.example', 
                                nexusUrl: '18.188.81.130:8081', 
                                nexusVersion: 'nexus3', 
                                protocol: 'http', 
                                repository: 'http://18.188.81.130:8081/repository/mavenrepo/', 
                                version: '1.0.0'

                    } 
        }
            }   
}
}

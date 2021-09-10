pipeline{ 
    agent any 
    tools {
        maven 'Maven3.8'
        
      }
    stages{
        stage("Checkout"){
            steps{
                echo "========Checking out code========"
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'MyGithubaccount', url: 'https://github.com/vcroshan/simple-java-maven-app.git']]])
                
            }
            post{
                success{
                    echo "========Code checkout completed successfully========"
                }
                failure{
                    echo "========Code Checkout failed========"
                }
            }
        }
        stage ("Execute script") {
            steps {
                sh '''
                echo "WORKSPACE: ${WORKSPACE}"
                echo "Jenkins Job Name: ${JOB_NAME}"
                '''
            }
        }
        stage ("Build"){
            steps{
                
                sh "mvn -B -DskipTests clean package"
            }
            }
        stage ("Test"){
            steps{
                
                sh "mvn test"
            }
            post {
                success {
                    junit "target/surefire-reports/**/*.xml"
                }
            }
           
            }
        /*stage ("sonar scanning") {
            steps {
                script { 
                    def scannerHome = tool name: 'mySonarScanner';
                    withSonarQubeEnv("mySonarqubeServer") {
                        sh "${tool("mySonarScanner")}/bin/sonar-scanner \
                        -Dsonar.projectKey=SimpleMaven \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://44.197.132.246:9000/ \
                        -Dsonar.login=12e2853ff7e7d0dd75ce2666eca6699aa3dc6d0a"
                    }
               }
            }
        }*/
        stage ("Upload to Nexus") {
            steps {
                sh "mvn -gs ${WORKSPACE}/settings.xml deploy"
               }
            }
        
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
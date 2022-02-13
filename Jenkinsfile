pipeline{
   agent {
       label "mybuildserver"
   }
    tools {
        maven 'maven'
    }
    stages{
        stage("code checkout"){
            steps{
                echo "========checking out code from github repo========"
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '2d64d980-832f-4dd0-b22d-b5cb971e0a7a', url: 'https://github.com/vcroshan/simple-java-maven-app.git']]])
            }
            post{
                success{
                    echo "========Code checkout from Github repo completed========"
                }
                failure{
                    echo "========Code checkout from Github repo failed========"
                }
            }
        }
        stage ("execute script") {
            steps{
                echo "Workspace:- $WORKSPACE"
                echo "Job Name :- $JOB_NAME"
                echo "Build ID :- $BUILD_ID"
                echo "Jenkins Home :- $JENKINS_HOME"
                echo "Inputparam1 : $Inputparam1"
                echo "InputParam2 : $Inputparam2"
            }
        }
        stage("Build") {
            steps{
                sh 'mvn -DskipTests clean package'
            }
            post{
                success {
                    echo "=========Build completed successfully============="
                    
                }
                failure {
                    echo "==========Build failed=========="
                }
            }
        }
        stage("Unit Testing") {
            steps{
                sh 'mvn test'
            }
            post {
                success{
                    echo "======Unit testing completed successfull, publishing report========="
                    junit 'target/surefire-reports/*.xml'
                }
                failure{
                    echo "==========unit test cases failed, report not published===="
                }
            }
        }
        stage ("sonar scanning") {
            steps {
                script { 
                    //def scannerHome = tool name: 'mySonarScanner';
                    withSonarQubeEnv("MySonarqube") {
                        sh "${tool("mySonarscanner")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.10.15:9000 \
                        -Dsonar.login=cc178140ffe774764ca39f4c5f009e8756719923"
                    }
               }
            }
        }
        stage ("Upload to Nexus") {
            steps {
                sh "mvn -gs ${WORKSPACE}/settings.xml dependency:purge-local-repository deploy"
               }
            }

        
    
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}

pipeline{
    tools{
        maven 'mymaven'
    }

    agent any

    stages{
        

        stage("code Build"){
            steps{
                sh 'mvn clean install'
            }
        }

        stage("code scan"){
            steps{
                script { 
                    //def scannerHome = tool name: 'mySonarScanner';
                    withSonarQubeEnv("MySonarqube") {
                        sh "${tool("mysonarscanner")}/bin/sonar-scanner \
                        -Dsonar.projectKey=simple-java-maven-app \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target \
                        -Dsonar.host.url=http://172.31.1.242:9000 \
                        -Dsonar.login=sqa_97617600e00d3a770feb15a8cd696d1b25b1c9c6"
                    }
               }
            }
        }

     /*   stage("Upload artifact"){
            steps{
                script{
                    try{
                        sh 'mvn -s settings.xml deploy'
                    }catch(error){
                        echo error.message
                    }
                }
                
            }
        } */

        stage("Pipeline Success"){
            steps{
               echo "========pipeline succcessfully completed========"
                echo "========pipeline succcessfully ========"
            }
        }
    }
}

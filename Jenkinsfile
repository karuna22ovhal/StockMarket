pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {

        stage('Initialize'){
                def dockerHome = tool 'mydocker'
                env.PATH = "${dockerHome}:${env.PATH}"
        }

        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
         stage('Publish') {
             steps {
                echo 'Starting to build docker image!!'

                script {
                    // withDockerRegistry([credentialsId: 'karuna22ovhal', url: 'docker.io/karuna22ovhal']) {
                        def customImage = docker.build("karuna22ovhal/stocknew:latest")
                        customImage.push()
                    // }
                  
                }
            }
        }
    }
}

pipeline {
    agent {
        docker {
           // label 'docker'
            image 'node:latest'
            args '-p 3000:3000'
        }
    }
    tools { nodejs "node" }
           stage('Tests') {
               
            steps {
//                 script {
//                    docker.image('node:10-stretch').inside { c ->
                        echo 'Building..'
                         sh 'npm install'
                        echo 'Testing..'
                         //sh 'npm test'
                         sh 'npm version'
//                         sh "docker logs ${c.id}"
//                    }
//                 }
            }
        }
        stage('Build and push docker image') {
            steps {
                script {
                    def dockerImage = docker.build("shtlamrut/node-demo:master")
                    docker.withRegistry('', 'demo-docker') {
                        dockerImage.push('master')
                    }
                }
            }
        }
        stage('Deploy to remote docker host') {
            environment {
                DOCKER_HOST_CREDENTIALS = credentials('demo-docker')
            }
            steps {
                script {
//                     sh 'docker login -u $DOCKER_HOST_CREDENTIALS_USR -p $DOCKER_HOST_CREDENTIALS_PSW 127.0.0.1:2375'
                    sh 'docker pull shtlamrut/node-demo:master'
                    //sh 'docker stop node-demo'
                    sh 'docker rm node-demo'
                    sh 'docker rmi shtlamrut/node-demo:current'
                    sh 'docker tag shtlamrut/node-demo:master shtlamrut/node-demo:current'
                    sh 'docker run -d --name node-demo -p 3000:3000 shtlamrut/node-demo:current'
                }
            }
        }
    }

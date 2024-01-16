pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.8.6'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    node {
                        docker.image("python:${PYTHON_VERSION}-slim").inside("-v ${WORKSPACE}:/app") {
                            sh 'pip install -r requirements.txt'
                        }
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    node {
                        try {
                            docker.image("python:${PYTHON_VERSION}-slim").inside("-v ${WORKSPACE}:/app") {
                                sh 'python -m unittest discover'
                            }
                        } catch (e) {
                            echo "Test Stage Failed!"
                        } finally {
                            junit '**/test-reports/*.xml'
                        }
                    }
                }
            }
        }
    }
}

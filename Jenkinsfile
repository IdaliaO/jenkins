pipeline {
    agent any  // Usar cualquier nodo disponible de Jenkins
    environment {
        DOCKER_IMAGE = 'idaliao/myapp:latest'  // Nombre de la imagen Docker que se va a construir
    }
    stages {
        // Etapa para obtener el código desde el repositorio de GitHub
        stage('Checkout SCM') {
            steps {
                // Utiliza las credenciales de Git para clonar el repositorio
                checkout scm
            }
        }

        // Etapa para construir la imagen Docker
        stage('Build Docker Image') {
            steps {
                script {
                    // Ejecutar el build de Docker para construir la imagen
                    echo "Building Docker image..."
                    sh 'docker build -t $DOCKER_IMAGE .'  // Asegúrate de tener un Dockerfile en el repositorio
                }
            }
        }

        // Etapa para iniciar sesión en DockerHub y subir la imagen
        stage('Login to DockerHub') {
            steps {
                script {
                    // Usamos las credenciales de Docker que configuraste en Jenkins
                    withCredentials([usernamePassword(credentialsId: 'git', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Iniciar sesión en DockerHub usando las credenciales
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        // Etapa para subir la imagen Docker a DockerHub
        stage('Push Docker Image') {
            steps {
                script {
                    // Subir la imagen construida a DockerHub
                    echo "Pushing Docker image..."
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        // Etapa para limpiar imágenes no utilizadas después del build
        stage('Clean up') {
            steps {
                script {
                    // Limpiar imágenes que no se usan después de la subida
                    echo "Cleaning up Docker images..."
                    sh 'docker rmi $DOCKER_IMAGE'
                }
            }
        }
    }
    
    // Manejo de postacciones (puedes agregar notificaciones o auditoría aquí)
    post {
        success {
            echo 'Pipeline successfully completed!'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            cleanWs()  // Limpiar el espacio de trabajo después de la ejecución
        }
    }
}

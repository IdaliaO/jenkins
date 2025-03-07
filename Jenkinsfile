pipeline {
    agent any
    triggers {
        githubPush()  // Esto disparará el pipeline cuando GitHub envíe un push
    }
    stages {
        stage('Build') {
            steps {
                echo 'Construyendo el proyecto...'
                // Aquí puedes agregar los pasos de construcción
            }
        }
        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
                // Aquí puedes agregar los pasos de pruebas
            }
        }
        stage('Deploy') {
            steps {
                echo 'Desplegando la aplicación...'
                // Aquí puedes agregar los pasos de despliegue
            }
        }
    }
}


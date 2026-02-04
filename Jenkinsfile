pipeline {
    agent any
    environment {
        WEBSERVER = 'Apache'
    }
    stages {
        stage('Create web directory')
        {
            steps {
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
                //Fisrt, drop the directory if exists
                pwsh 'rmdir -Force -r C:/proyectos/jenkins/directivas/web-when'
                //Create the directory
                pwsh 'mkdir C:/proyectos/jenkins/directivas/web-when'
            }
        }
        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'droping the container...'
                pwsh 'docker rm -f apache1'
            }
        }
        stage('Crear contenedor apache') {
            when {
                environment name: 'WEBSERVER', value: 'Apache'
            }
            steps {
                echo 'Creating the container...'
                pwsh 'docker run -dit --name app-web -p 9100:80  -v C:/proyectos/jenkins/directivas/web-when:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Crear contenedor nginx') {
            when {
                environment name: 'WEBSERVER', value: 'nginx'
            }
            steps {
                echo 'Creating the container...'
                pwsh 'docker run -dit --name app-web -p 9100:80  -v C:/proyectos/jenkins/directivas/web-when:/usr/share/nginx/html nginx'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                pwsh 'cp -r web/* C:/proyectos/jenkins/directivas/web-when'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                pwsh 'Invoke-WebRequest -UseBasicParsing http://localhost:9000'
            }
        }
    }
}

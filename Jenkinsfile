pipeline {
    agent any

    parameters {
        choice choices: ['Apache', 'nginx'], name: 'WEBSERVER'
        choice choices: ['Development', 'Test', 'Production'], name: 'ENV'
    }

    stages {
        stage('Create web directory') {
            when {
                equals(actual: currentBuild.number, expected: 1)
            }
            steps {
                //Fisrt, drop the directory if exists
                pwsh 'rmdir -Force -r C:/proyectos/jenkins/directivas/web-when'
                //Create the directory
                pwsh 'mkdir C:/proyectos/jenkins/directivas/web-when'
            }
        }

        stage('Drop the Docker container') {
            steps {
                echo 'droping the container...'
                pwsh 'docker rm -f app-web'
            }
        }

        stage('Crear contenedor apache') {
            when {
                allOf {
                    environment name: 'WEBSERVER', value: 'Apache'
                    environment name: 'ENV', value: 'Development'
                }
            }
            steps {
                echo 'Creating the container...'
                pwsh 'docker run -dit --name app-web -p 9100:80  -v C:/proyectos/jenkins/directivas/web-when:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Error message Apache') {
        }

        stage('Crear contenedor nginx') {
            when {
                environment name: 'WEBSERVER', value: 'nginx'

                anyOf {
                    environment name: 'ENV', value: 'Test'
                    environment name: 'ENV', value: 'Production'
                }
            }
            steps {
                echo 'Creating the container...'
                pwsh 'docker run -dit --name app-web -p 9100:80  -v C:/proyectos/jenkins/directivas/web-when:/usr/share/nginx/html nginx'
            }
        }

        stage('Error message nginx') {
            when {
                allOf {
                    environment name: 'WEBSERVER', value: 'nginx'
                    environment name: 'ENV', value: 'Development'
                }
            }
            steps {
                echo 'Nginx no se puede desplegar en dev'
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
                pwsh 'Invoke-WebRequest -UseBasicParsing http://localhost:9100'
            }
        }
    }
}

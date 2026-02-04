pipeline {
    agent any
   
    stages {
        stage('Create web directory')
        {
            input {
                message 'Enter the data'
                parameters {
                    string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment ')
                    string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
                }
            }
            steps{
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
                //Fisrt, drop the directory if exists
                pwsh "rmdir -Force -r C:/proyectos/jenkins/directivas/web"
                //Create the directory
                pwsh 'mkdir C:/proyectos/jenkins/directivas/web'
                
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
                echo 'droping the container...'
                pwsh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
                echo 'Creating the container...'
                pwsh 'docker run -dit --name apache1 -p 9000:80  -v C:/proyectos/jenkins/directivas/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                pwsh 'cp -r web/* C:/proyectos/jenkins/directivas/web'
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

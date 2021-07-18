pipeline {
    agent any 

    triggers {
        pollSCM('* * * * *')
    }
    // Got permission denied while trying to connect to the Docker daemon socket at unix.
    // sudo usermod -a -G docker jenkins
    // restart jenkins server ->  sudo service jenkins restart
    stages {
        
        stage('Maven Compile') {
            steps {
                echo '----------------- This is a compile phase ----------'
                bat 'mvn clean compile'
            }
        }
        
        stage('Maven Build') {
             steps {
                echo '----------------- This is a build phase ----------'
                bat 'mvn clean package -DskipTests'
            }
        }
		
		stage('Docker Build') {
            steps {
                echo '----------------- This is a build docker image phase ----------'
                bat '''
                    docker image build -t ecom-webservice .
                '''
            }
        }

       stage('Docker Deploy') {
            steps {
                echo '----------------- This is a docker deploment phase ----------'
                bat '''
                 (if  [ $(docker ps -a | FINDSTR ecom-webservice | cut -d " " -f1) ]; then \
                        echo $(docker rm -f ecom-webservice); \
                        echo "---------------- successfully removed ecom-webservice ----------------"
                     else \
                    echo OK; \
                 fi;);
            docker container run --restart always --name ecom-webservice -p 8090:8090 -d ecom-webservice
            '''
            }
        }
    }
}

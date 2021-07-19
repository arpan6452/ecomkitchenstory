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
		
		stage('Maven Deploy') {
            steps {
                echo '----------------- This is a deploment phase ----------'
                bat '''
                    java -jar ./target/spring_boot_jwt-0.0.1-SNAPSHOT.jar
                '''
            }
        }
    }
}
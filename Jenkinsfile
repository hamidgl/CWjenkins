pipeline {
    agent any

  environment {
    registry = "hamidgl/CW2-3"
    registryCredential = 'dockerhub'
  }

    stages {
	
	stage('Cloning Git') {
  steps {
    git 'https://github.com/hamidgl/CW2-3.git'
  }
}
        stage('Build') {
            steps {
			  script {
         dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
            }
        }
		
		stage('Deploy Image') {
  steps{
    script {
      docker.withRegistry( '', registryCredential ) {
        dockerImage.push()
      }
    }
  }
}
		
        stage('Sonarqube') {
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=sonar-js -Dsonar.sources=." 
        }
    }
}

	stage('update application'){
		
		steps{
		sleep (10)
		sh 'ssh azureuser@40.76.45.129 kubectl set image deployments/CW2-3 CW2-3=hamidgl/CW2-3:$BUILD_NUMBER'
		
		}
}

    }
}


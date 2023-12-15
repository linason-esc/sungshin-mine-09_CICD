pipeline {
    agent any
    environment {
        PROJECT_ID = 'openai-398712' 
        CLUSTER_NAME = 'opensourceteam6'
        LOCATION = 'asia-northeast3-a'
        CREDENTIALS_ID = '43529197-a0c0-489c-876a-2ea1cfc75263'
    }
    stages {
        stage('Clone repository') {
			steps {
				git branch:'main',
				url:'https://github.com/linason-esc/sungshin-mine-09_CICD.git'
			}
	    }
	    stage('Build image') {
			steps {
				script {
					app = docker.build("opensourceteam6/opensourceteam6")
				}
			}
	    }
	    stage('Push image') {
			steps {
				script {
					docker.withRegistry('https://registry.hub.docker.com', 'opensourceteam6') {
			    		app.push("${env.BUILD_NUMBER}")
			    		app.push("latest")
		   			}
				}
			}
	    }
        stage('Deploy to GKE') {
            when {
                branch 'main'
            }
            steps {
                sh "sed -i 's/opensourceteam6:latest/opensourceteam6:${env.BUILD_NUMBER}/g' dep.yml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, 
                clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'dep.yml', credentialsId: env.CREDENTIALS_ID,
                verifyDeployments: true])
            }
        }
    }
}
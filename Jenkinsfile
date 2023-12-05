node {
	def app
	stage('Clone repository') {
		git branch:'main',
			url:'https://github.com/linason-esc/sungshin-mine-09_CICD.git'
	}
	stage('Build image') {
		app = docker.build("opensourceteam6/opensourceteam6")
	}

	stage('Push image') {
		docker.withRegistry('https://registry.hub.docker.com', 'opensourceteam6') {
			app.push("${env.BUILD_NUMBER}")
			app.push("latest")
		}
	}
}

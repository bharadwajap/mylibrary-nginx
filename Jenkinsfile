#!groovy
def dockerAgent = 'docker-slave'

def branch
def projectName = 'mylibrary-nginx'
def gitCredentials = 'mylibrary-github'

// pipeline
node(dockerAgent) {

	try {
		stage('Collect info') {
			checkout scm
			branch = env.BRANCH_NAME
		}

		stage('Dockerize') {
			final String activeContainers = sh(script: "sudo docker ps", returnStdout: true)
			boolean containerFound = activeContainers.toLowerCase().contains("${projectName}")
			if (containerFound) {
				sh "sudo docker stop ${projectName}"
				sh "sudo docker rm ${projectName}"
			}
			sh "sudo docker build -t ${projectName} ."
			sh "sudo docker run --network=host --restart always --name=${projectName} -td ${projectName}"
		}
	} catch (def e) {
		print "Exception occurred while running the pipeline"+ e
	} finally {
		deleteDir()
	}
}

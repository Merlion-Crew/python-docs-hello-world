node {
   stage('Checkout Code') {
      checkout scm
   }
   stage('Push Docker Images to Nexus Registry') {
      echo "Username: ${params.docker_username}"
      echo "Password: ${params.docker_password}"
      echo "Logging into the docker registry\n"
      sh "docker login -u ${params.docker_username} -p ${params.docker_password} 52.175.54.34:8123"
      echo "Building the docker image\n"
      sh "docker build -t python-docs-hello-world ."
      echo "Tagging the image\n"
      sh "docker tag python-docs-hello-world:latest 52.175.54.34:8123/python-docs-hello-world:latest"
      sh "docker push 52.175.54.34:8123/python-docs-hello-world:latest"
    }
}
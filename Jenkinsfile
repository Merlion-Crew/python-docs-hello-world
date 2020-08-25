node {
   stage('init') {
      checkout scm
   }
   stage('Push Docker Images to Nexus Registry') {
    echo "User: $user"
    echo "Passwd: $pass"
    sh 'docker login -u admin -p NexusAdmin08 52.175.54.34:8123'
    sh 'docker build -t python-docs-hello-world .'
    sh 'docker tag python-docs-hello-world:latest 52.175.54.34:8123/python-docs-hello-world:latest'
    sh 'docker push 52.175.54.34:8123/python-docs-hello-world:latest'
    //sh 'docker rmi $(docker images --filter=reference="NexusDockerRegistryUrl/ImageName*" -q)'
    //sh 'docker logout NexusDockerRegistryUrl'
    }
}
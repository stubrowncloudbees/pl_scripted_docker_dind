def label = "pl_scripted_docker_dind-${UUID.randomUUID().toString()}"
def image_name = "stuartcbrown/jentest:${label}"
podTemplate(label: label,
        containers: [
            containerTemplate(name: 'docker', image: 'docker:17.12.1-ce-dind', privileged: true)
            ]
        ) {
    node(label) {
        container("docker") {
            stage("docker") {
                checkout scm
                echo image_name
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]) {


                    sh 'docker login -p ${PASSWORD} -u ${USER}'
                    sh "docker build . -t $image_name"
                    sh "docker push $image_name"
                    sh "sleep 200"

                }
            }
        }
    }
}


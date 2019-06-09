node {
    dir("/root/"){
    checkout scm

    env.DOCKER_API_VERSION="1.23"
    appName = "default/flask-app"
    registryHost = "10.0.0.1:8500/"
    imageName = "${registryHost}${appName}:${env.BUILD_ID}"
    env.BUILDIMG=imageName
    docker.withRegistry('https://10.0.0.1:8500/', 'docker'){
    stage "Build"

        def pcImg = docker.build("mycluster.icp:8500/default/flask-app:${env.BUILD_ID}", "-f Dockerfile.ppc64le .")
        sh "cp /root/.dockercfg ${HOME}/.dockercfg"
        pcImg.push()

    input 'Do you want to proceed with Deployment?'
    stage "Deploy"

        sh "kubectl set image deployment/demoapp-demochart demochart=${imageName}"
        sh "kubectl rollout status deployment/demoapp-demochart"
}
}
}


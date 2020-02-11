# devopsweb
CI/CD for simple HTML webpage

# Jenkins pipeline Script

node {
    stage('Checkout') 
    {
        checkout([$class: 'GitSCM', 
            branches: [[name: '*/master']], 
            doGenerateSubmoduleConfigurations: false, 
            extensions: [], 
            submoduleCfg: [], 
            userRemoteConfigs: [[url: 'https://github.com/pramodksahoo/devopsweb.git']]])   
    }

    stage('Build image') 
    {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("staticweb:latest")
    }

    stage('Deployment')
    {
        sh 'docker rm -f stacikweb'
        sh 'docker run -itd --name=stacikweb -p 81:80 staticweb:latest'
    }
}

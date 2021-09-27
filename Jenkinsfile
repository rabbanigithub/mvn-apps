node {
    stage('SCM Checkout'){
        git branch: 'main', url: 'https://github.com/rabbanigithub/mvn-apps'
    }
    stage('MVN Package'){
        sh 'mvn clean package'
    }
    stage('Build Docker Image'){
        sh 'docker build -t rabbanidocker/mvn-apps .'
    }
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pass', variable: 'dockerHubPass')]) {
            sh "docker login -u rabbanidocker -p ${dockerHubPass}"
            }
        sh 'docker push rabbanidocker/mvn-apps'
    }
    stage('Deploy on Dev Server'){
        def dockerRun = 'docker run -p 8080:8080 -d --name mvn-apps rabbanidocker/mvn-apps'
        sshagent(['6a534226-e2cb-41a5-bd5b-24427216b285']) {
            sh "ssh -o StrictHostKeyChecking=no vagrant@10.11.12.81 ${dockerRun}"
        }
    }
}

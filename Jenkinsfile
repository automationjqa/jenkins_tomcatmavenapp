node {
    
    notify('Started')
    try {
        stage('checkout') {
        git 'https://github.com/annajel/TomcatMavenApp.git'
        }
        
        stage('compile, test, package') {
            withMaven(maven: 'maven'){
            sh label: '', script: 'mvn clean package jelastic:deploy'
            }
        }
        
        stage('archival') {
            archiveArtifacts 'target/*.war'
        }
        
        notify('Success')
        
    } catch (err) {
        notify("Error ${err}")
        echo "Caught: ${err}"
        currentBuild.result = 'FAILURE'
    }
}

def notify(status){
    emailext (
      from: "jenkins@cluster.com",
      to: "ash@mailhog.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}



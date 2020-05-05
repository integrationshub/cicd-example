pipeline
{
	agent { docker { image 'my_local_maven' } }
	
	stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
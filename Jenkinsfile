pipeline
{
	agent {
		any {
			label 'my_label'
		}
	}
	
	tools {
        maven 'my_local_maven'
    }
    
	stages {
	
		stage ('Build') {

		    git url: 'https://github.com/cyrille-leclerc/multi-module-maven-project'
		    
		    steps{
		
			/*
		    withMaven(
		    
		        // Maven installation declared in the Jenkins "Global Tool Configuration"
		        maven: 'my_local_maven',
		        
		        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
		        // We recommend to define Maven settings.xml globally at the folder level using 
		        // navigating to the folder configuration in the section "Pipeline Maven Configuration / Override global Maven configuration"
		        // or globally to the entire master navigating to  "Manage Jenkins / Global Tools Configuration"
		        mavenSettingsConfig: 'my-maven-settings') {
		
		      // Run the maven build
		      sh "mvn clean verify"
		
		    } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe & FindBugs & SpotBugs reports...
		    */
		    
		    
		    
		    withMaven(
		    	maven: 'my_local_maven',
		    	mavenSettingsConfig: 'my-maven-settings'
		    ) {
		    	sh "mvn clean verify"
		    }
		    
		    
		    
		    /*
		    withMavenSettingsXml(mavenSettingsConfig: 'enterprise-settings',
                                 image: config.mavenImage,
                                 options: [artifactsPublisher(disabled: true)]) {
                def PROJECT_VERSION=sh(returnStdout: true, script: "mvn help:evaluate -Dexpression=project.version").trim()
                echo "publishing ${PROJECT_VERSION} to artifactory"
                sh "mvn clean source:jar install deploy"
            }
            */
            
		    }
		    
		  }
		
		
	}
	
	
	
	post {
        always {
            echo 'This will always run'
            
        }
        success {
            echo 'This will run only if successful'
             
        }
        failure {
            echo 'This will run only if failed'
             
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
    
    
}
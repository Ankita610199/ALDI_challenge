pipeline 
{
    agent any
   

    stages {
        stage('Get Source Code') {
            steps {
                git credentialsId: '291a3c49-fa9f-409c-ad87-cae9c219fcf6', url: 'https://github.com/mgmkamran/testjavaapplication.git'
                echo 'Hello World'
            }
        }
        stage('Build code') {
            steps
            {
                sh script: 'mvn package'
            }
        }
               
        stage('Run Test')
        {
            steps
            {
                sh script: 'mvn test -Dbrowser=localchrome'
            }
        }
        stage('Publish Report')
        {
            steps
            {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: '', reportFiles: 'target/surefire-reports/Extent*.html', reportName: 'Pipeline', reportTitles: ''])
            }
        }
        stage("sonar Quality  Check")
        {
           steps
           {
                withSonarQubeEnv(installationName: 'sonar-9', credentialsId: 'Jenkins-sonar-token')
        	      {
                        sh 'mvn sonar:sonar -Dsonar.projectKey=Aldi'
                   }
		   }
		}
		stage('Run Integration test')
        {
            steps
                {
                    sh script: 'mvn test -Dbrowser=localchrome'
                }
        }
		      /*  {
	                steps
	                {
			               def mvn = tool 'Default Maven'
		                   withSonarQubeEnv(installationName: 'sonar-9', credentialsId: 'Jenkins-sonar-token')
						      {
		                          bat "%mvn%/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=Aldi"
		                      }
				   }
				}*/
		stage("Publish to Nexus Repository Manager") 
		{
            steps 
            {
                 nexusArtifactUploader artifacts: [
				[artifactId: 'pom.artifactId',
				classifier: '',
				file: 'pom.xml',
				type: 'pom.xml'
				]
				],
				credentialsId: 'jenskins-nexus-user',
				groupId: 'pom.groupId', 
				nexusUrl: '20.196.196.47:8081', 
				nexusVersion: 'nexus3', 
				protocol: 'http', 
				repository: 'nexus-rep-projects',
				version: '1.0-SNAPSHOT'
                        
            }
        }
        stage('Deploy to QA') 
        {
    	    steps
    	    {
    		    sh label: '', script: 'scp /var/lib/jenkins/workspace/Testproject/webapp/target/webapp.war   azureuser@10.2.0.5:/var/lib/tomcat9/webapps/qa.war'
    	    }
	    }
	    
	    stage('Deploy to PROD') 
        {
    	    steps
    	    {
    		    sh label: '', script: 'scp /var/lib/jenkins/workspace/Testproject/webapp/target/webapp.war   azureuser@10.4.0.4:/var/lib/tomcat9/webapps/prod.war'
    	    }
	    }
        
       
    }
}
                
    

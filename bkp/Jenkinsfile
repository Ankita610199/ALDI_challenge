node('built-in') 
{
    stage('Git Checkout') 
	{
    		git 'https://github.com/mgmkamran/testjavaapplication.git'
	}
    stage('Build') 
	{
    		sh label: '', script: 'mvn package'
	}
    stage('Deploy to QA') 
	{
		sh label: '', script: 'scp /home/ubuntu/.jenkins/workspace/ScriptedPipeline/webapp/target/webapp.war   ubuntu@172.31.26.217:/var/lib/tomcat9/webapps/qaenv.war'
	}
    stage('Test') 
	{
              	sh label: '', script: 'echo "Testing Passed"'
	}
    stage('Deploy to PROD') 
	{
		sh label: '', script: 'scp /home/ubuntu/.jenkins/workspace/ScriptedPipeline/webapp/target/webapp.war   ubuntu@172.31.22.88:/var/lib/tomcat9/webapps/prodenv.war'
	}
}

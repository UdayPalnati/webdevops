/*
1. Setup Maven path in Manage Jenkins >>  Global Tool Configuration >> under Maven >> 
	Name: maven-3.8.3
	Value: maven home path (ex: /usr/lib/maven/apache-maven-3.8.1)
2. Setup Git path in Manage Jenkins >>  Global Tool Configuration >> under Git >> 
	Name: Default
	Value: Git home path (ex: /usr/bin/git )
3. Make sure jenkins master or node have the lable - build. (OR) update the lable name you want in the below code.
*/


node("build"){
	stage('checkout'){
		checkout scm
	}
	stage('compile'){
		
		sh"${tool 'maven-3.8.3'}/bin/mvn -V clean compile -DreleaseVersion=1.0.${BUILD_NUMBER}"
	}
	stage('junit test'){
		sh"${tool 'maven-3.8.3'}/bin/mvn -V clean test -DreleaseVersion=1.0.${BUILD_NUMBER}"
	}
	stage('deploy-to-nexus'){
    		print 'deploy the package to nexus'
		//sh"${tool 'maven-3.8.3'}/bin/mvn -V clean deploy -DreleaseVersion=1.0.${BUILD_NUMBER}" //this command is not deploying to nexus, install nexus and update the command to deploy
		//sh '"/root/apache-maven-3.8.3/bin/mvn" -V clean deploy'
	}
	stage('deploy-to-tomcat'){
		println 'deploy the package to tomcat server to run application'
		
		sh'''
			echo "Removing the existing package from tomcat server"
			ssh http://172-31-33-236:rm -rf $HOME/tomcat9/webapps/DevOpsWebApp*
			
			echo "Deploy(copy) war to tomcat server"
			scp target/DevOpsWebApp*.war http://172-31-33-236:$HOME/tomcat9/webapps/

		'''
		
	
		sh '''
			echo Deploy the war to tomcat server.

			echo Step-1: Removing the existing package
			rm -rf /root/tomcat9/webapps/DevOpsWebApp-*.war
			rm -rf /root/tomcat9/webapps/DevOpsWebApp-*

			echo Step-2: Staging the new package to tomcat server.
			cp ${WORKSPACE}/target/DevOpsWebApp-*.war /root/tomcat9/webapps
		'''
		
	}
}

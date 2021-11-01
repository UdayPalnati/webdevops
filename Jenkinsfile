/*
1. Setup Maven path in Manage Jenkins >>  Global Tool Configuration >> under Maven >> 
	Name: maven-3.8.1
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
    		println 'deploy the package to nexus'
		sh"${tool 'maven-3.8.3'}/bin/mvn -V clean deploy -DreleaseVersion=1.0.${BUILD_NUMBER}" //this command is not deploying to nexus, install nexus and update the command to deploy
		sh '"/root/apache-maven-3.5.4/bin/mvn" -V clean deploy'
	}
	
			
}

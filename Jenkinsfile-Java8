pipeline {
    agent any
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git url: 'https://github.com/tahseenintelli/sample-java-app'
		sh script: '/opt/maven/bin/mvn compile'
           }
        }
        stage('codereview-pmd') {
	   steps {
                echo 'codereview..'
		sh script: '/opt/maven/bin/mvn -P metrics pmd:pmd'
           }
	   post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        sh script: '/opt/maven/bin/mvn test'
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
	    
	stage('code-coverage') {
	   steps {
                echo 'code coverage..'
	        sh script: '/opt/maven/bin/mvn cobertura:cobertura -Dcobertura.report.format=xml'
                 }
	   post {
               success {
                   cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
               }
           }			
        }
	    
        stage('package') {
	   steps {
                echo 'package......'
		sh script: '/opt/maven/bin/mvn package'	
           }		
        }
    }
}

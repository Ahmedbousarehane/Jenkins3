	pipeline {
	    agent any
	    tools {
	        maven 'apache-maven-3.5.2'
        	jdk 'jdk1.8.0_151'
	    }
	    stages {
	        
	        stage ('Build') {
	            steps {
					bat 'mvn install'
	            }
	           
	        }
	        stage ('unit test') {
	            steps {
					bat 'mvn compile'
	            }
	            post {
	                success {
	                    junit 'target/surefire-reports/*.xml' 
	                }
	            }
	        }

			stage('cobertura'){
				steps{
					bat 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
				}
				post{
					success {
						step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/*.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
					}
	}
			}
			
			stage('package'){
				steps{
					bat 'mvn package'
				}

			}
			
			             stage('Analyse statique') {
                 steps{
                        bat 'mvn checkstyle:checkstyle findbugs:findbugs sonar:sonar -Dsonar.host.url=http://sic.emi.ac.ma:9000'
                        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/checkstyle-result.xml', unHealthy: ''
                         findbugs canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '**/findbugsXml.xml', unHealthy: ''

                 }
}
		    
			}
	}

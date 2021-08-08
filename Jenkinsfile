pipeline{
	agent{label 'master'}
	environment {
        	FLASK_DEBUG=1
		FLASK_APP="flasky.py"
		registry = "hub.docker.com/anjurose" 
	        registryCredential = 'HubID1' 
	        dockerImage = '' 
    	}
	stages{
		stage('Checkout'){
			steps{
				git branch: 'master', url: 'https://github.com/AnjuMeleth/flasky.git'
			}
		}
    		stage('Setup'){
      			steps{
				sh 'chmod +x install.sh'
        			sh './install.sh'
				//sh 'python --version' 
				//sh 'python -m pip install --upgrade pip'
				//sh 'python --version' 
      			}
    		}
    		stage('Test'){
      			steps{
				 sh '''#!/bin/bash
				     source myprojectenv/bin/activate	
                		     python -m unittest
				     '''
               			}
   		}
		stage('Build image'){
			steps{
				dockerImage = docker.build registry + ":$BUILD_NUMBER" 
			}
		}
		stage('Deploy our image') { 
    		        steps { 
		               docker.withRegistry( '', registryCredential ) { 
		               dockerImage.push() 
			       }
                	} 
	            }
		stage('Run'){
      			steps{
				sh '''#!/bin/bash
				     source myprojectenv/bin/activate 
				     flask run --host 0.0.0.0
				     '''
      			}
   		}
	}
}
			

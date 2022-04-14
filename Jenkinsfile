pipeline {
	    agent any
	    environment {
	        ORCHESTRATOR_URL = "https://cloud.uipath.com/applebots/Finance/orchestrator_/"
	        ORCHESTRATOR_LOGICAL_NAME = "applebots"
	        ORCHESTRATOR_TENANT_NAME = "Finance"
	        ORCHESTRATOR_FOLDER_NAME = "Shared"
	    }
	    stages {
		 
	         // Building the package
	        stage('Build') {
	            steps {
	            echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      traceLevel: 'None',
			      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                    //  version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
			      version: CurrentVersion(),
	                      useOrchestrator: false
						  
	        )
	            }
	        }
	         

	         // Deploying
	        stage('Deploy to Orchestrator') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to Orchestrator "
	                UiPathDeploy (
	                packagePath: "Output\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${ORCHESTRATOR_URL}",
	                orchestratorTenant: "${ORCHESTRATOR_TENANT_NAME}",
	                folderName: "${ORCHESTRATOR_FOLDER_NAME}",
	               environments: '',
	                credentials: Token(accountName: "${ORCHESTRATOR_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
			traceLevel: 'None',
			entryPointPaths: 'Main.xaml'
	
	        )
	            }
	        }
	    }
	
	    
	    post {
	        success {
	            echo 'Orchestrator Deployment is sucessfully completed!'
	        }
	        failure {
	          echo "Deployment Failed : Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	                cleanWs()
	        }
	    }
	

	}

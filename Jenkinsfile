pipeline{
		agent{
			
			kubernetes {
			    
			    yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: bc15-helm
            image: alpine/helm
            command:
            - cat
            tty: true
        '''
    
    }
		}
    environment {
        MY_KUBECONFIG = credentials('config-file')
    }

		stages{
			
	stage ('BC15GC-FE') {
        steps {
            // Freestyle build trigger calls a list of jobs
            // Pipeline build() step only calls one job
            // To run all three jobs in parallel, we use "parallel" step
            // https://jenkins.io/doc/pipeline/examples/#jobs-in-parallel
		
		    build job: 'BC15GC-FE'
		
        }
    }
				
                stage('Checkout Source') {
		      steps {
			git 'https://github.com/PDhanrajnath/gc'
		      }   
                        }
                   
                    
                stage("Build") {
                        steps {
                            container('bc15-helm'){
                               sh """
                                  export KUBECONFIG=\${MY_KUBECONFIG}
                                  
                                  helm upgrade --install bc15 . -n bc15                              
                                  """
                            //     sh "export KUBECONFIG=\${config} helm repo update --kubeconfig=$MY_KUBECONFIG"
                            //   sh "export KUBECONFIG=\${config} helm install voting myvoteapp --kubeconfig=$MY_KUBECONFIG"
                            }
                            
                        }
                    }
                    
                    
				
                
            }
        }

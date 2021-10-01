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
              stage ('BC15GC-FE') {
        	steps {
		
		    build job: 'BC15GC-FE', parameters: [string(name: 'master', value: env.BRANCH_NAME)]
		
        }
    }
                    
				
                
            }
        }

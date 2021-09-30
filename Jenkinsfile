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
				
                stage("add Repo") {
                        steps {
                            container('bc15-helm'){
                               sh "helm repo add  bc15gc https://pdhanrajnath.github.io/gc/ "
                            }
                            
                        }
                    }
                    
                stage("Build") {
                        steps {
                            container('bc15-helm'){
                               sh """
                                  export KUBECONFIG=\${MY_KUBECONFIG}
                                  helm repo update 
                                  helm upgrade --install bc15 bc15gc/bc15-gc -n bc15                              
                                  """
                            //     sh "export KUBECONFIG=\${config} helm repo update --kubeconfig=$MY_KUBECONFIG"
                            //   sh "export KUBECONFIG=\${config} helm install voting myvoteapp --kubeconfig=$MY_KUBECONFIG"
                            }
                            
                        }
                    }
                    
                    
				
                
            }
        }

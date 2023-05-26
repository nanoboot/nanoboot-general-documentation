pipeline 
{
    agent any
    environment { 
    	DEFAULT_LATEST_OUT_DIR = 'latest/en-US/html'
    	DEFAULT_SNAPSHOT_OUT_DIR = 'snapshot/en-US/html'
    	DOC_NAME = 'nanoboot-general-documentation'
    }
    stages
    {
        stage('Build')
        {
            steps {
            
	            echo "*** Building ${env.JOB_NAME} ***"
    		    sh '''
    		        #!/bin/bash
    		        echo JOB_NAME=$JOB_NAME
    		        
    		        if [ -z "$DOG_HOME" ]
                        then
                              echo "KO : Variable DOG_HOME is empty. You fix this issue by adding this variable to Jenkins"
                              exit 1
                        else
                              echo "OK : Variable DOG_HOME is NOT empty"
                        fi
                    
                    alias dog='java -jar $DOG_HOME/dog-*-jar-with-dependencies.jar'
                    
    		        dog
    		        echo "Build of documentation was successful"
    		        '''
            }
        }
        
        stage('Deploy')
        {
            steps {
                echo "*** Deploying ${env.JOB_NAME} ***"
              
    		    sh '''
    		        #!/bin/bash
    		        
    		        case $BRANCH_NAME in

    		          master)
                        echo Branch $BRANCH_NAME is supported. Continuing.
                        OUT_DIR=$DEFAULT_LATEST_OUT_DIR
        		        ;;
    		        
      		          develop | jenkins)
        		        echo Branch $BRANCH_NAME is supported. Continuing.
                        OUT_DIR=$DEFAULT_SNAPSHOT_OUT_DIR
        		        ;;
    		        
      		        *)
        		        echo Branch $BRANCH_NAME is not supported. A failure happened. Exiting.
                        exit 1
        		        ;;
    		        esac

    		        if [ -z "$DOC_NAME" ]
                        then
                              echo "KO : Variable DOC_NAME is empty. You fix this issue by adding this variable to section 'environment' of this Jenkinsfile"
                              exit 1
                        else
                              echo "OK : Variable DOC_NAME is NOT empty"
                        fi
                        
                    if [ -z "$DOCS_ROOT_DIR" ]
                        then
                              echo "KO : Variable DOCS_ROOT_DIR is empty. You fix this issue by adding this variable to Jenkins configuration"
                              exit 1
                        else
                              echo "OK : Variable DOCS_ROOT_DIR is NOT empty"
                        fi
                        
                    if [ -z "$DOCS_ROOT_DIR" ]
                        then
                              echo "KO : Variable DOCS_ROOT_DIR is empty. You fix this issue by adding this variable to Jenkins configuration"
                              exit 1
                        else
                              echo "OK : Variable DOCS_ROOT_DIR is NOT empty"
                        fi
                    
                    echo "OUT_DIR=$OUT_DIR"
    		        FULL_OUTPUT_DIR=$DOCS_ROOT_DIR/$DOC_NAME/$OUT_DIR
    		        if [ -z "$FULL_OUTPUT_DIR" ]
                        then
                              echo "KO : Variable FULL_OUTPUT_DIR is empty"
                              exit 1
                        else
                              echo "OK Variable FULL_OUTPUT_DIR is NOT empty"
                              rsync -vaz ./generated/ $FULL_OUTPUT_DIR
                              echo "Deployment of documentation was successful"
                        fi
    		       '''
	          
            }
        }
    }
}


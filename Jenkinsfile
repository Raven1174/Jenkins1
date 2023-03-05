
pipeline {
  agent any
 parameters {
        string(name: 'name_container', defaultValue: 'portainer', description: 'nombre del docker')
        string(name: 'name_imagen', defaultValue: 'portainer/portainer-ce', description: 'nombre de la imagen')
        string(name: 'tag_imagen', defaultValue: 'latest', description: 'etiqueta de la imagen')
    }
    environment {
        name_final = "${name_container}"        
    }
    stages {
          stage('stop/rm') {

            when {
                expression { 
                    DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=${name_final})"').trim()
                    return  DOCKER_EXIST != '' 
                }
            }
            steps {
                script{
                    sh ''' 
                         docker stop ${name_final}
                    '''
                    }
                    
                }                    
                                  
            }
           
        stage('build') {
            steps {
                script{
                    sh ''' 
                    docker pull ${name_imagen}:${tag_imagen}
                    '''
                    }
                    
                }                    
                                  
            }
            stage('run') {
            steps {
                script{
                    sh ''' 
                        docker run -it -v /etc/localtime/:etc/localtime -v $HOME/.rainboow_oauth:root/.rainbow_oauth -v $HOME/.rainbow_config.json:/root/.rainbow_config.json --name ${name_final} ${name_imagen}
                        docker run -d -p 8000:8000 -p 9443:9443 --name ${name_final} --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data ${name_imagen}:${tag_imagen}
                    '''
                    }
                    
                }                    
                                  
            }
            
          
        }   
    }

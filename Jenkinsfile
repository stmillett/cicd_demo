node ('ubuntu-agent'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
   /* stage('SAST'){
        build 'SECURITY-SAST-SNYK'
    }*/

    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("stmillett/snake")
    }
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'Dockerhub') {
            app.push("latest")
        			}
         }
    /*stage('SECURITY-IMAGE-SCANNER'){
        build 'SECURITY-IMAGE-SCANNER-AQUAMICROSCANNER'
    }*/
  
   stage("Generate Software Bill of Materials (sbom) with Syft"){
	sh 'pwd'
        sh '''
            //curl -sSfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b /usr/local/bin
            syft app:${BUILD_NUMBER} --scope all-layers -o json > sbom-${BUILD_NUMBER}.json
            syft app:${BUILD_NUMBER} --scope all-layers -o table > sbom-${BUILD_NUMBER}.txt
        '''
    			
	} 
    stage('Pull-image-server') {
    
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
    
    /*stage('DAST')
        {
        build 'SECURITY-DAST-OWASP_ZAP'
        }*/
 
}

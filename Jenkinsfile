node ("docker"){
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("tese/nginx:v1")
    }

    
    stage('Test image') {
      sh 'export GOSS_FILES_STRATEGY=cp && /usr/local/bin/dgoss  run -p 80 --name dgoss-test --rm -ti tese/nginx:v1'
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://docker-registry-default.apps.paosin.local', 'docker-registry-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}

node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALSS', url: 'https://github.com/manikarnam/kubernetes-bootstrapper.git', branch: 'master'
    }
	
	stage('Whoami'){
	  sh "whoami"
		
	}
    
    /**stage('Build Docker Image'){
	    sh "sudo docker build -t maniengg/spring-boot-mongo:${BUILD_ID} ."
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "sudo docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        }
        sh "sudo docker push maniengg/spring-boot-mongo:${BUILD_ID}"
     }
     
    /** stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'mykubeconfig',
         enableConfigSubstitution: true
        )
     }**/
	 
     stage("Deploy To Kuberates Cluster"){
        withCredentials([file(credentialsId: 'demo-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         sh "gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
         sh "gcloud config set project mssdevops-284216"
         sh "gcloud config set compute/zone us-central1-c"
         sh "gcloud config set compute/region us-central1"
         sh "gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project mssdevops-284216"
	 sh "sudo snap install helm --classic"
        // sh "kubectl create serviceaccount --namespace kube-system tiller"
         sh "kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller"
         sh "kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'"
         sh "helm init --service-account tiller --upgrade"
        // sh "sed -i -e 's,image_to_be_deployed,'maniengg/spring-boot-mongo:${BUILD_ID}',g' springBootMongo.yml"
        /** sh "kubectl apply -f monitoring/namespace.yml"
         sh "kubectl apply -f helm/role-binding.yml"    
         sh "kubectl apply -f helm/service-account.yml"
         sh "kubectl apply -f prometheus/values.yml"
         sh "kubectl apply -f monitoring/grafana/config.yml"
         sh "kubectl apply -f monitoring/grafana/values.yml" **/
         
        }
      }
}

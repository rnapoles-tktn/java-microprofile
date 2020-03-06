
podTemplate(label: 'label', cloud: 'openshift', serviceAccount: 'kabanero-pipeline', containers: [
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat',
                      envVars: [ envVar(key: 'TAG', value: 'latest'),
                                envVar(key: 'IMAGENAME', value: 'java-microprofile'),
                               envVar(key: 'PROJECT', value: 'kabanero')])
  ]){
    node('label') {
        stage('Deploy') {
            container('kubectl') {
                checkout scm
                sh 'sed -i -e \'s#applicationImage: .*$#applicationImage: image-registry.openshift-image-registry.svc:5000/\'$PROJECT\'/\'$IMAGENAME\':\'$TAG\'#g\' app-deploy.yaml'
                sh 'cat app-deploy.yaml'
                sh 'find . -name app-deploy.yaml -type f|xargs kubectl apply -f'
            }
        }   
    }    
}

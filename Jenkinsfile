podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
        containers:
        - name: gradle 
          image: gradle:jdk8 
          command:
          - sleep
          args:
          - 99d
          restartPolicy: Never
    '''){
node(POD_LABEL) {
    stage('Kubernetes on gradle container') {
    git 'https://github.com/slykmh/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
    container('gradle') {
      stage('start calculator') {
        sh '''
        cd Chapter08/sample1
        curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
        chmod +x ./kubectl
        ./kubectl apply -f calculator.yaml
        ./kubectl apply -f hazelcast.yaml
        '''
        }
      stage('testing'){
        sh '''
        test $(curl calculator-service:8080/div?a=6\\&b=0) -eq 6 && echo 'pass' || 'fail'
        '''
        }
      } 
    }
  }
}

podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos:latest
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
''') {
      node(POD_LABEL) {
            stage('k8s') {
              git 'https://github.com/mp125uml/week9.git'
              container('centos') {
                stage('deploy calculator') {
                    sh '''
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    chmod +x ./kubectl
                    ./kubectl apply -f calculator.yaml -n staging
                    ./kubectl apply -f hazelcast.yaml -n staging
                    '''
                }
                stage('test calculator') {
                    sh '''
                    curl 'calculator-service:8080/sum?a=1&b=2'
                    curl 'calculator-service:8080/div?a=6&b=3'
                    '''
                }
              }
            }
      }
    }

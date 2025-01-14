# tekton-demo
This is for the tekton demo -Docker Bangalore


kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097

Task.yaml

````
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: echo-task
spec:
  params:
    - name: greeting
      type: string
      default: "Hello, Tekton!"  
  steps:
    - name: echo
      image: ubuntu
      script: |
        #!/bin/bash
        echo "$(inputs.params.greeting)"


```

Pipeline.yaml

```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: hello-pipeline
spec:
  params:
    - name: greeting
      type: string
      default: "Hello, Tekton!"  
  tasks:
    - name: echo-task
      taskRef:
        name: echo-task
      params:
        - name: greeting
          value: "$(params.greeting)"  


```
Pipelinerun.yaml

```

apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  name: hello-pipelinerun
spec:
  pipelineRef:
    name: hello-pipeline
  params:
    - name: greeting
      value: "Greetings from Tekton!"



```



```

tkn pipelinerun logs hello-pipelinerun


```


pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn clean install'  
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                sh 'scp target/my-app.jar user@server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}


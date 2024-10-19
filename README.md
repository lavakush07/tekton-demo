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
      default: "Hello, Tekton!"  # Default value
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
      default: "Hello, Tekton!"  # Default value
  tasks:
    - name: echo-task
      taskRef:
        name: echo-task
      params:
        - name: greeting
          value: "$(params.greeting)"  # Pass the parameter to the task


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


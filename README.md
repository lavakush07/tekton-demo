# tekton-demo
This is for the tekton demo -Docker Bangalore

````
task.yaml
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

If you need to perform a task after something has is finished, you can use an exit handler. Exit handlers are specified using `onExit` :

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: exit-handler-
spec:
  entrypoint: main
  templates:
    - name: main
      dag:
        tasks:
          - name: a
            template: whalesay
            onExit: tidy-up

    - name: whalesay
      container:
        image: docker/whalesay
        command: [ cowsay ]

    - name: tidy-up
      container:
        image: docker/whalesay
        command: [ cowsay ]
        args: [ "tidy up!" ]
```

They just state the name of the template that should be run on-exit.

Lets look at a complete example:

`cat exit-handler-workflow.yaml`

Run it:

`argo submit --watch exit-handler-workflow.yaml`

You should see:

```php
STEP                   TEMPLATE  PODNAME                        DURATION  MESSAGE
 ✔ exit-handler-plvg7  main                                                 
 ├─✔ a                 whalesay  exit-handler-plvg7-1651124468  5s          
 └─✔ a.onExit          tidy-up   exit-handler-plvg7-3635807335  6s          
```

Note how the exit handler task ran last.

## Exercise

An exit handler can be run at the end of a template, or at the end of a workflow. Change the example to run one at the end of the workflow.

Learn more about [exit handlers](https://argoproj.github.io/argo-workflows/walk-through/exit-handlers/), as well as their close cousin, [lifecycle hooks](https://argoproj.github.io/argo-workflows/lifecyclehook/), in the Argo Workflows documentation.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: exit-handler-
spec:
  entrypoint: main
  onExit: tidy-up
  templates:
    - name: main
      dag:
        tasks:
          - name: a
            template: whalesay
            
    - name: whalesay
      container:
        image: docker/whalesay
        command: [ cowsay ]

    - name: tidy-up
      container:
        image: docker/whalesay
        command: [ cowsay ]
        args: [ "tidy up!" ]
```

```php
STEP                          TEMPLATE  PODNAME                                 DURATION  MESSAGE
 ✔ exit-handler-gmqwn         main                                                          
 └─✔ a                        whalesay  exit-handler-gmqwn-whalesay-1986847462  5s          
                                                                                                     
 ✔ exit-handler-gmqwn.onExit  tidy-up   exit-handler-gmqwn-tidy-up-4047273830   6s      
```

![[Pasted image 20230204192622.png]]

Depending on the success of a workflow

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: exit-handler-
spec:
  entrypoint: main
  onExit: tidy-up
  templates:
    - name: main
      dag:
        tasks:
          - name: a
            template: whalesay

    - name: whalesay
      container:
        image: docker/whalesay
        command: [ cowsay ]

    - name: tidy-up
      steps:
      - - name: celebrate
          template: celebrate
          when: "{{workflow.status}} == Succeeded"
        - name: cry
          template: cry
          when: "{{workflow.status}} != Succeeded"

    - name: celebrate
      container:
                image: docker/whalesay
                command: [ cowsay ]
                args: [ "tidy up!" ]
    - name: cry
      container:
            image: docker/whalesay
            command: [ cowsay ]
            args: [ "oh no!" ]
```

```php
STEP                          TEMPLATE   PODNAME                                  DURATION  MESSAGE
 ✔ exit-handler-j9vp7         main                                                                                                         
 └─✔ a                        whalesay   exit-handler-j9vp7-whalesay-3090167688   6s                                                       
                                                                                                                                                    
 ✔ exit-handler-j9vp7.onExit  tidy-up                                                                                                      
 └─┬─✔ celebrate              celebrate  exit-handler-j9vp7-celebrate-3166996485  4s                                                       
   └─○ cry                    cry                                                           when 'Succeeded != Succeeded' evaluated false      
```

![[Pasted image 20230204195459.png]]


Workflow templates have a different `kind` to a workflow, but are otherwise very similar:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: WorkflowTemplate
metadata:
  name: hello
spec:
  entrypoint: main
  templates:
    - name: main
      container:
        image: docker/whalesay
        command: [ cowsay ]
```

Let's create this workflow template:

`argo template create hello-workflowtemplate.yaml`

You can also manage templates using `kubectl` :

`kubectl apply -f hello-workflowtemplate.yaml`

This allows you to use GitOps to manage your templates.

To submit a template, you can use the UI or the CLI:

`argo submit --watch --from workflowtemplate/hello`

You should see:

```
STEP            TEMPLATE  PODNAME      DURATION  MESSAGE
 ✔ hello-c622t  main      hello-c622t  33s         
```

Lets take a look at the workflow you created:

`argo get @latest -o yaml`

Look for the workflow specification in the output:

```yaml
spec:
  workflowTemplateRef:
    name: hello
```

Note how the specification of the workflow is actually a reference to the template.

## Exercise

-   Use the user interface to submit a workflow template:
    
    -   Port-forward to the Argo Server pod...
    
    `kubectl -n argo port-forward --address 0.0.0.0 svc/argo-server 2746:2746 > /dev/null &`
    
    -   and [open the Argo Workflows UI](https://70923c5e-7ec7-4e2b-ad6f-2ce5e6bd3afa-10-244-5-8-2746.saci.r.killercoda.com/workflows/argo?limit=50).
-   Update the workflow template to add some parameters (e.g. to print a message). Use `argo submit --from` to submit it with different parameters.
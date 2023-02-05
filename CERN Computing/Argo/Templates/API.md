## [[Access Token ]]

If you want to automate tasks with the Argo Server API or CLI, you will need an Access Token.

## [[Webhooks]]

Many clients can send events via the [events](https://argoproj.github.io/argo-workflows/events/) API endpoint using a standard authorization header. However, for clients that are unable to do so (e.g. because they use signature verification as proof of origin), additional configuration is required.

In the namespace that will receive the event, create [access token](https://argoproj.github.io/argo-workflows/access-token/) resources for your client:

-   A role with permissions to get workflow templates and to create a workflow: [example](https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start/base/webhooks/submit-workflow-template-role.yaml)
-   A service account for the client: [example](https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start/base/webhooks/github.com-sa.yaml).
-   A binding of the account to the role: [example](https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start/base/webhooks/github.com-rolebinding.yaml)

Additionally create:

-   A secret named `argo-workflows-webhook-clients` listing the service accounts: [example](https://raw.githubusercontent.com/argoproj/argo-workflows/master/manifests/quick-start/base/webhooks/argo-workflows-webhook-clients-secret.yaml)

The secret `argo-workflows-webhook-clients` tells Argo:

-   What type of webhook the account can be used for, e.g. `github`.
-   What "secret" that webhook is configured for, e.g. in your Github settings page.

## API Docs

You can find API docs in the user interface, try it now:

[Open the API docs](https://08d872e4-802d-471c-9e66-dc2a16a6f374-10-244-5-9-2746.saci.r.killercoda.com/apidocs)

You can use the API docs to send API requests, so it is really useful to test things out.

But you were asked for a password weren't you?

Use your `ARGO_TOKEN` as a password:

`echo $ARGO_TOKEN`

Copy the whole response, including `Bearer` , and paste it into the white box in the center of the Argo UI Login page where it says "client authentication".

Once you're logged in, you may need to click to open the API docs again:

[Open the API docs](https://08d872e4-802d-471c-9e66-dc2a16a6f374-10-244-5-9-2746.saci.r.killercoda.com/apidocs)

## Exercise

Open the API docs and find an endpoint to create workflow templates. Create a workflow template and then submit it.

## More API documentation

Visit the Argo Workflows documentation for more information:

-   [Argo Workflows API](https://argoproj.github.io/argo-workflows/rest-api/)
-   [Access Tokens](https://argoproj.github.io/argo-workflows/access-token/)
-   [API examples](https://argoproj.github.io/argo-workflows/rest-examples/)
-   [API reference (Swagger docs)](https://argoproj.github.io/argo-workflows/swagger/)
-   [Client libraries for Python, Golang, and Java](https://argoproj.github.io/argo-workflows/client-libraries/)
-   [Argo Server](https://argoproj.github.io/argo-workflows/argo-server/)
[Argo Workflows Course](https://killercoda.com/pipekit/course/argo-workflows)

# [[Templates]]

There are several types of templates, divided into two different categories: **work** and **orchestration**.

The first category defines **work** to be done. This includes:

-   Container
-   Container Set
-   Data
-   Resource
-   Script

The second category **orchestrates** the work:

-   DAG
-   Steps
-   Suspend

Next, we're going to take a deep-dive into the two most common template types are Container and DAG Templates.

# [[Inputs and Outputs]]

Let's recap:

-   There are two types of input and output: **parameters** and **artifacts**.
-   **Parameters** are just short strings.
-   **Artifacts** are simply files or directories that are compressed and uploaded to an **artifact repository** such as S3.
-   For one task to use the output of another task, declare a dependency using a DAG template or steps template.

Learn more about artifact features in the Argo Workflows documentation:

-   [Artifacts overview](https://argoproj.github.io/argo-workflows/walk-through/artifacts/)
-   [Configuring an artifact repository](https://argoproj.github.io/argo-workflows/configure-artifact-repository/)
-   [Key-only artifacts](https://argoproj.github.io/argo-workflows/key-only-artifacts/)
-   [Referencing artifact repositories](https://argoproj.github.io/argo-workflows/artifact-repository-ref/)

# [[Reuse]]

Let's recap:

-   A **workflow template** is a workflow that can be reused or as a library item.
-   A **cron workflow** is a workflow that runs on a schedule.

# Using the [[API]]

## Info Endpoint

The Argo Server provides the API. This is secured using Kubernetes service accounts.

All endpoints can be found under `http://localhost:2746/api/v1` URL and typically require an **access token**.

To access this via Killercoda, we need to port-forward the argo-server pod:

`kubectl -n argo port-forward --address 0.0.0.0 svc/argo-server 2746:2746 > /dev/null &`

Note: Killercoda currently has issues port-forwarding. You may need to wait a few minutes for Killercoda to complete the port-forward.

Typically, it is good to be able to check you can access it first. This can be done using the `info`endpoint:

`curl http://localhost:2746/api/v1/info`

You should see something like this if it is successful:

```json
{"code":16,"message":"token not valid for running mode"}
```

If it fails, then you'll see something like this:

```yaml
curl: (7) Failed to connect to localhost port 2746: Connection refused
```

Note: If see a failure message, it is likely a Killercoda issue. You should try re-submitting the `kubectl -n argo port-forward` command listed above, and then repeat the steps to confirm your port-forward was successful before moving to the next section.

To connect, we need to set-up an **access token.**


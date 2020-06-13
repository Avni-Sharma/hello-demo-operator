Coonect to a cluster. 
export KUBECONFIG=~/.kubeconfig

For local development:
Run `operator-sdk run --local --watch-namespace=default`

In another terminal apply all files under deploy/* except the cr.yaml
The operator is now watching for our custom `Hello` resource.

Now apply the deploy/crds/*cr.yaml

The hello instance is applied.
The output-

```
avni@localhost hello-demo-operator (master)=>>   operator-sdk run --local --watch-namespace=default
INFO[0000] Running the operator locally in namespace default.
{"level":"info","ts":1592054141.296003,"logger":"cmd","msg":"Operator Version: 0.0.1"}
{"level":"info","ts":1592054141.2960336,"logger":"cmd","msg":"Go Version: go1.14.3"}
{"level":"info","ts":1592054141.2960405,"logger":"cmd","msg":"Go OS/Arch: linux/amd64"}
{"level":"info","ts":1592054141.2960463,"logger":"cmd","msg":"Version of operator-sdk: v0.17.0"}
{"level":"info","ts":1592054141.297297,"logger":"leader","msg":"Trying to become the leader."}
{"level":"info","ts":1592054141.2973094,"logger":"leader","msg":"Skipping leader election; not running in a cluster."}
{"level":"info","ts":1592054144.7841344,"logger":"controller-runtime.metrics","msg":"metrics server is starting to listen","addr":"0.0.0.0:8383"}
{"level":"info","ts":1592054144.784828,"logger":"cmd","msg":"Registering Components."}
{"level":"info","ts":1592054144.784938,"logger":"cmd","msg":"Skipping CR metrics server creation; not running in a cluster."}
{"level":"info","ts":1592054144.784965,"logger":"cmd","msg":"Starting the Cmd."}
{"level":"info","ts":1592054144.7851338,"logger":"controller-runtime.manager","msg":"starting metrics server","path":"/metrics"}
{"level":"info","ts":1592054144.785173,"logger":"controller-runtime.controller","msg":"Starting EventSource","controller":"hello-controller","source":"kind source: /, Kind="}
{"level":"info","ts":1592054145.1857433,"logger":"controller-runtime.controller","msg":"Starting EventSource","controller":"hello-controller","source":"kind source: /, Kind="}
{"level":"info","ts":1592054145.4862413,"logger":"controller-runtime.controller","msg":"Starting Controller","controller":"hello-controller"}
{"level":"info","ts":1592054145.486376,"logger":"controller-runtime.controller","msg":"Starting workers","controller":"hello-controller","worker count":1}
{"level":"info","ts":1592054215.4967723,"logger":"controller_hello","msg":"Reconciling Hello","Request.Namespace":"default","Request.Name":"example-hello"}
{"level":"info","ts":1592054215.7973185,"logger":"controller_hello","msg":"Creating a new Deployment","Request.Namespace":"default","Request.Name":"example-hello","Deployment.Namespace":"default","Deployment.Name":"example-hello"}
{"level":"info","ts":1592054216.1532829,"logger":"controller_hello","msg":"Reconciling Hello","Request.Namespace":"default","Request.Name":"example-hello"}
{"level":"info","ts":1592054216.6065102,"logger":"controller_hello","msg":"Reconciling Hello","Request.Namespace":"default","Request.Name":"example-hello"}
{"level":"info","ts":1592054216.8841972,"logger":"controller_hello","msg":"Reconciling Hello","Request.Namespace":"default","Request.Name":"example-hello"}
```

Now check for the status field in the hello instance by running `kubectl get hello example-hello -o yaml`

```
apiVersion: hello.example.com/v1alpha1
kind: Hello
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"hello.example.com/v1alpha1","kind":"Hello","metadata":{"annotations":{},"name":"example-hello","namespace":"default"},"spec":{"size":3}}
  creationTimestamp: "2020-06-13T13:16:55Z"
  generation: 1
  name: example-hello
  namespace: default
  resourceVersion: "25974"
  selfLink: /apis/hello.example.com/v1alpha1/namespaces/default/hellos/example-hello
  uid: df856c5d-455c-4761-b5d1-7c2777ca6d0e
spec:
  size: 3
status:
  nodes:
  - example-hello-6499cfd547-7vdjf
  - example-hello-6499cfd547-rl8gt
  - example-hello-6499cfd547-cs6cv
  ```

  The status is updated with the pod names. This is in accordance with our reconcile logic.
# Muhammad Sean Arsha Galant 2206822963
## Tutorial 11 Part 1
## Reflection
1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?
- Before exposing it as a Service
![Screenshot 2024-05-13 193310.png](assets%2FScreenshot%202024-05-13%20193310.png)

- After exposing it as a Service and opening the app several times while the proxy into the Service is running
![Screenshot 2024-05-13 194216.png](assets%2FScreenshot%202024-05-13%20194216.png)

As observed from the two logs above, it increases from before exposing it as a Service and after exposing it as a Service. We can see that the number of logs increase each time the app is accessed using `kubectl logs hello-node-55fdcd95bf-grt6p`, and in this case the logs increases by 2 every time we access the app. This is because the logs are to specify two
GET requests that occur; one for the HTTP GET at port 8080, and one for UDP GET on port 8081. As seen from the logs above, I tried accessing the app 3 times, hence there are 6 logs for GET requests in total.

2. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

The purpose of the `-n` option stands for `--namespace`. In Kubernetes, namespaces provide a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced objects.
In this case, `-n` allows you to specify the namespace in which you want to perform the operation. Regarding why the output did not list the explicitly created pods or services, it's the pod `hello-node` was created in a namespace other than `kube-system`, that is the `default` namespace for docker, 
and the command did not specify to list resources from all namespaces. For instance, the command `kubectl get pods` without specifying a namespace retrieves pods from the `default` namespace, and we are able to observe the `hello-node` pod. If our explicitly created pods are in a different namespace, they wouldn't be listed unless we specify that namespace using the `-n` option.


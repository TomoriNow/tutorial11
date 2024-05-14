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

## Tutorial 11 part 2
## Reflection

1. What is the difference between Rolling Update and Recreate deployment strategy?

Rolling Update and Recreate are two distinct deployment strategies in Kubernetes, each serving specific purposes. Rolling Update, the default strategy, gradually replaces existing pods with updated ones, ensuring continuous availability of the application throughout the update process. It offers control over the update rate and enables automatic rollback in case of issues, making 
it suitable for production environments where minimizing downtime is crucial. In contrast, Recreate terminates all existing pods before creating new ones, resulting in downtime during the update. For Rolling Update, the deployment updates pods in a rolling update fashion when the field `.spec.strategy.type==RollingUpdate`. You can specify `maxUnavailable` and `maxSurge` fields to control the rolling update process.
Conversely, for Recreate, all existing pods are killed before new ones are created when the field `.spec.strategy.type==Recreate`. 

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.

- Creating the Spring Petclinic REST deployment
![Screenshot 2024-05-14 125314.png](assets%2FScreenshot%202024-05-14%20125314.png)
![Screenshot 2024-05-14 125322.png](assets%2FScreenshot%202024-05-14%20125322.png)
![Screenshot 2024-05-14 125529.png](assets%2FScreenshot%202024-05-14%20125529.png)

- Creating the yaml files from the created deployment
![Screenshot 2024-05-14 131450.png](assets%2FScreenshot%202024-05-14%20131450.png)

- Deleting and Restarting Minikube
![Screenshot 2024-05-14 131808.png](assets%2FScreenshot%202024-05-14%20131808.png)

- Editing `delpoyment-recreate.yaml` to have the Recreate deployment strategy and add 4 replicas for the deployment
![Screenshot 2024-05-14 132441.png](assets%2FScreenshot%202024-05-14%20132441.png)

- Testing out the Recreate deployment strategy in action
![Screenshot 2024-05-14 132745.png](assets%2FScreenshot%202024-05-14%20132745.png)
![Screenshot 2024-05-14 132823.png](assets%2FScreenshot%202024-05-14%20132823.png)
![Screenshot 2024-05-14 133203.png](assets%2FScreenshot%202024-05-14%20133203.png)

As observed in the testing images, when setting the deployment to a new deployment image (updated version 3.2.1 from 3.0.2), it stops the current running the deployment and recreates the pods (Containers) from the beginning to redeploy it. When we roll back the version to 3.0.2, it terminates all the existing pods and recreates them again according to the strategy.

3. Prepare different manifest files for executing Recreate deployment strategy.
Found in the files above: `deployment-recreate.yaml` and `services-recreate.yaml`

4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply-f` command) to the cluster.

Using Kubernetes manifest files offers significant advantages over manual deployment. They ensure consistency and reproducibility by defining the desired state of resources declaratively, which simplifies scaling and updates. Stored in version control, these files facilitate 
tracking changes, auditing, and collaboration. They integrate well with CI/CD pipelines, automating deployments and reducing human error. Additionally, manifest files provide clear, up-to-date documentation of the infrastructure. Overall, they enhance efficiency, reliability, and ease of management compared to manual deployment methods.
Compared to deploying the app manually in the command line through a series of several cli commands and steps, it is much simpler to have a predefined manifest file and use that immediately to our deployment by using the `kubectl apply -f` command due to automation and ease of use that comes with these manifest files for deployment.










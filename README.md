## **Harness Lab Assignment**

**1 - Kubernetes setup -- Time taken: 12m**

I already had minikube installed. I tried to run it and found it was broken:

```
bash-3.2$ minikube start
There is a newer version of minikube available (v1.12.3).  Download it here:
https://github.com/kubernetes/minikube/releases/tag/v1.12.3

To disable this notification, run the following:
minikube config set WantUpdateNotification false
😄  minikube v1.0.1 on darwin (amd64)
🤹  Downloading Kubernetes v1.14.1 images in the background ...
💡  Tip: Use 'minikube start -p <name>' to create a new cluster, or 'minikube delete' to delete this one.
🔄  Restarting existing virtualbox VM for "minikube" ...

💣  Unable to start VM: start: Error setting up host only network on machine start: The host-only adapter we just created is not visible. This is a well known VirtualBox bug. You might want to uninstall it and reinstall at least version 5.0.12 that is is supposed to fix this issue
```

I found some removal steps and ran these:
```
minikube stop; minikube delete &&
docker stop $(docker ps -aq) &&
rm -rf ~/.kube ~/.minikube &&
sudo rm -rf /usr/local/bin/localkube /usr/local/bin/minikube &&
launchctl stop '*kubelet*.mount' &&
launchctl stop localkube.service &&
launchctl disable localkube.service &&
sudo rm -rf /etc/kubernetes/ &&
docker system prune -af --volumes
```

Then I used brew install to install it and we are up and running cleanly:

**Screenshot 1 (MiniKube):**
![Alt text](/Step_1_-_Minikube.png?raw=true "Minikube")

**2 - Harness Trial and Delegate Install -- Time taken: 11m**

I followed the setup for the YAML instructions, installed this and verified it was running properly:

**Screenshot 2 (Delegate):**
![Alt text](/Step_2_-_Delegate.png?raw=true "Delegate")

**3 - Service, Environments, and Workflow -- Time taken: 41m**

It took me a little bit to realize I needed to create an application to do this properly. Once I figured that out, I tried to create the Infrastructure Definition and ran into this error:

```
Invalid request: Invalid request: No eligible delegates could perform the required capabilities for this task: [ DELEGATE_NAME:harness-sample-k8s-delegate ] - The capabilities were tested by the following delegates: [ doc-example-bgjnoe-0 ] - Following delegates were validating but never returned: [ ] - Other delegates (if any) may have been offline or were not eligible due to tag or scope restrictions.
```

Googleing this error wasn't insightful at first. I dug a bit deeper and then realized I hadn't properly setup the Cloud Provider.

This solved the issue and I was able to move on with creating the other pipelines and deployments properly.

**Screenshot 3 (Service):**
![Alt text](/Step_3_-_Service.png?raw=true "Service")

**4 - Pipelines and Templatization -- Time taken: 7m**

Once I found the little square button with a T in it, this was straightforward. I then recreated a generalized pipeline to run this under and set it up

**Screenshot 4 (Pipelines):**
![Alt text](/Step_4_-_Pipelines.png?raw=true "Pipelines")

**Feedback**

-The link to 'Install The Harness Delegate' has been moved and should be updated within the UI
-It wasn't clear that you need to create an 'application' and I found this in Setup, Create Application. It seems like the wizard and/or the instructions should be a bit more clear about this creation step.
-A knowledge base article could be written on the error I received in Step 3


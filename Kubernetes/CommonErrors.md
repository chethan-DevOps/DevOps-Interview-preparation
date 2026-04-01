Common Kubernetes Errors :
CrashLoopBackOff Error
ImagePullBackOff Error, ErrImagePull
OOMKilled
CreateContainerConfig Error
CreateContainer Error
RunContainer Error
NodeDiskPressure
NodeNotReady

CreateContainerConfigError vs. CreateContainerError

While these two errors may sound alike, they happen at different stages in the container lifecycle:

CreateContainerConfigError: This happens when something is wrong with your Pod’s configuration. Think of it as a setup issue that stops the container from being created.

CreateContainerError: This occurs later, during the actual creation of the container. The setup might be correct, but the container fails to start for other reasons.

If a pod is terminate due to a memory issue. it doesn’t necessarily mean it will be removed from the node. If the node’s restart policy is set to ‘Always’, the pod will attempt to restart

# jenkins-slack-k8s

Configure a Jenkins pipeline on Kubernetes with Github and Slack

### Prerequisites
- 这里使用的是 a free IBM Cloud account.
	> Install the IBM Cloud command-line interface (CLI) to your work station.
- 本机 Mac 使用 Docker Desktop.
	> 同时 Create a Docker Hub account.
- Install a Kubernetes CLI (kubectl) on Mac
- Install a Git Client.
	> Sign up for a GitHub account.
- Create a Slack account.

### Key Procedure
- 验证是否可以连接到集群。 
	kubectl version --short
	> Client Version: v1.16.1
	Server Version: v1.14.9+IKS
- 获取 Jenkins dashboard 服务地址
	`export EXTERNAL_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="ExternalIP")].address }')`
	
	`export NODE_PORT=30100`
	
	`echo $EXTERNAL_IP:$NODE_PORT`
	> 184.172.229.55:30100
- 获取 Jenkins admin 默认密码
	> kubectl logs $(kubectl get pods --selector=app=jenkins -o=jsonpath='{.items[0].metadata.name}') jenkins

- 配置 Jenkins Slack Notification 主要填写 Workspace, Credential。Default channel / member id 可不填，具体可在 Jenkinsfile 配置里指定，比如
	> `success {
slackSend(channel: "#ok", message: "pluckhuang/podinfo:${env.BUILD_NUMBER} Pipeline is successfully completed.")}`


### Reference and resource
- [configure-a-cicd-pipeline-with-jenkins-on-kubernetes](https://developer.ibm.com/tutorials/configure-a-cicd-pipeline-with-jenkins-on-kubernetes/)
- [github repo for jenkins-slack-k8s](https://github.com/pluckhuang/jenkins-slack-k8s)
	
	

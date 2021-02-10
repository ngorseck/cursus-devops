# Prerequisites

## Amazon Web Services Educate

This tutorial leverages the [Amazon Web Services](https://aws.amazon.com/) and [Amazon Web Services Educate](https://aws.amazon.com/education/awseducate/?nc1=h_ls) to streamline provisioning of the compute infrastructure required to bootstrap GitOps environment.

> AWS Educate is an educational platform to start working with a cloud infrastructure. This platform operates AWS but not all AWS services are available as on the official platform.

## Amazon CloudFormation

[AWS CloudFormation](https://aws.amazon.com/cloudformation/provides) a common language for you to model and provision AWS and third party application resources in your cloud environment. CloudFormation allows you to use programming languages or a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications across all regions and accounts. This gives you a single source of truth for your AWS and non-AWS resources.

## Amazon Web Services Roles 

This project uses [AMI roles](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) as well and [instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) for EC2 instances

### IAM Role 

For security reasons, we use the principle of least privilege to define roles and comply. We create two roles with specific policies:
- **ec2RoleDescribeInstance** : One role to allow description of EC2 instances only 

### Instance Profiles role
We use this principle to attache role specially for desired instance
- The **ec2RoleDescribeInstance** is attached to Kubernetes worker nodes with the name **ec2InstanceProfileDescribeInstance**

## Description of projet

It is a  CloudFormation stack that provides a complete environment for practicing the GitOps concepts in the AWS educate environement . This uses AWS CloudFormation for infrastructure as code, IAM roles and Instance profiles.

This stack provides a complete environment with a private GitLab instance with certificate management for the container registry and a functional Kubernetes cluster with 3 nodes. One master and two workers.

You just have to train on the gitops :blush:

## Infrastructure 

![schema_infra](https://user-images.githubusercontent.com/58267422/87213138-0e55d800-c323-11ea-9c96-a1f63655cf7a.png)
<br>


# Deployment 

##  AWS Region 

In this tutorial, we will work exclusively in the AWS region ```us-east-1```. 
All the resources that will be deployed will be in this region.

## Operating System 

### Deployment instance

In this tutorial, we use a **Linux Debian 9 (stretch)** distribution as an environment to provision all of the resources in AWS provider.

### Resources Deployment

In this tutorial, we deploy EC2 instances of **Linux Debian 9 (stretch)** distribution.

## AWS Educate 

- After you login in your AWS Educate account, you arrive in the **AWS Management Console** web page : 

![0_main page](https://user-images.githubusercontent.com/58267422/87348192-a1298900-c554-11ea-9a15-9afebb18907e.png)


## SSH key pair

AWS use SSH with private and public key authentication. It's mandatory to create and use key pair to connect on EC2 instances.

### Create a SSH key pair

- From **AWS Management Console** web page, type **EC2** in the text area **Find services** and then click on EC2 : 

![1_AWS_Management_console_search_ec2](https://user-images.githubusercontent.com/58267422/87348392-efd72300-c554-11ea-9f88-d5bf817b81c6.png)

- In the left menu of the window, search for **Network & Security > Key Pairs** and click on **Key Pairs** :

![2_click_key_pairs](https://user-images.githubusercontent.com/58267422/87348772-915e7480-c555-11ea-895e-822fa61d213d.png)

- In top right of window click on **Create key pairs** :

![3_create_key_pairs](https://user-images.githubusercontent.com/58267422/87428503-b77e2600-c5e2-11ea-9a7a-a24be07ff9ea.png)

- Type the name of your key pair in the text area of the **Key pair** section, in this tutorial we name the key ```my-key-gitops``` and then click **Create key pair**, and then click on **Create key pair**  :

![4_create_my-key-gitops](https://user-images.githubusercontent.com/58267422/87349132-1f3a5f80-c556-11ea-9f31-23ec0627075b.png)

- You must have a green notification, to notify you **Successfully created key pair** :

![5_creation_success](https://user-images.githubusercontent.com/58267422/87349273-53158500-c556-11ea-92eb-c2fa1c9989c3.png)

**Note : The private key file is automatically generated by AWS with the following name ```my-key-gitops.pem```**
**It's very important we keep this private key in secure place and doesn't lose this file because AWS deliver this private key only one time** 

<br>

## Get CloudFormation stack

You can get AWS CloudFormation stack file at the following address :
```
https://github.com/samiamoura/gitops-training/blob/master/aws-educate-stack/stack-GitOps-educate.yml
```

- Copy and paste this stack file in your latop

<br>


## CloudFormation deployment 

- Go back in the **AWS Management Console** web page, type **CloudFormation** in the text area **Find services** and then click on CloudFormation :

![6_AWS Management console _cloudformation](https://user-images.githubusercontent.com/58267422/87350302-f5823800-c557-11ea-8a2a-41c9ac126594.png)

### Create a stack

- Click on **Create a stack** :

![7_create_stack](https://user-images.githubusercontent.com/58267422/87430978-3e80cd80-c5e6-11ea-8af5-cb60db651a31.png) 

### Specify template

- In the **Prerequisite - Prepare template** section, you can leave default choice **Template is ready**,

- In the **Specify model** section, select **Download a model file** and choose the AWS CloudFormation stack that we downloaded previously **stack-GitOps-educate.yml**, then click **Next**  :

![8_specify_template](https://user-images.githubusercontent.com/58267422/87351307-8a396580-c559-11ea-9b45-8aed2304edb4.png)


### Specify stack details

- In the **Stack name** section, type the name of the stack, in this tutorial we name the stack ```my-stack-gitops``` : 

![9_satck_name](https://user-images.githubusercontent.com/58267422/87426340-4d17b680-c5df-11ea-8573-ea6f748b8486.png)


#### ImageType correspond OS type of EC2 Instances, two types are allowed

- Debian GNU/Linux 9 (stretch) - ami-003f19e0e687de1cd
- CentOS Linux 7 (Core) - ami-0083662ba17882949

In this tutorial we use **Debian GNU/Linux 9 (stretch) - ami-003f19e0e687de1cd** 

![12_image type (debian)](https://user-images.githubusercontent.com/58267422/87431145-84d62c80-c5e6-11ea-96ba-a2cfac39790b.png)

Note : These Amazon Machine Images (AMI) are availables for AWS region **us-east-1**

#### InstanceTypeGitLab correspond type of EC2 Instances of **GitLab Community Edition**, numerous types are allowed

- t2.medium, t2.large, t2.xlarge, t2.2xlarge
- t3.nano, t3.micro, t3.small, t3.medium, t3.large, t3.xlarge, t3.2xlarge
- m4.large, m4.xlarge, m4.2xlarge, m4.4xlarge, m4.10xlarge
- m5.large, m5.xlarge, m5.2xlarge, m5.4xlarge
...

Recommanded value is mimimum **t2.medium** : 

![13_instancetype gitlab](https://user-images.githubusercontent.com/58267422/87351982-983bb600-c55a-11ea-8e22-829f0b85f9cb.png)


#### InstanceTypeKubernetesMaster correspond type of EC2 Instances for **Kubernetes master node**, numerous type are allowed
- t2.medium, t2.large, t2.xlarge, t2.2xlarge
- t3.nano, t3.micro, t3.small, t3.medium, t3.large, t3.xlarge, t3.2xlarge
- m4.large, m4.xlarge, m4.2xlarge, m4.4xlarge, m4.10xlarge
- m5.large, m5.xlarge, m5.2xlarge, m5.4xlarge
...

Recommanded value is mimimum **t2.medium** : 

![14_instancetype_k8s_master](https://user-images.githubusercontent.com/58267422/87352065-b99ca200-c55a-11ea-883c-4b38d9e7f7f5.png)


#### InstanceTypeKubernetesWorker correspond type of EC2 Instances for **Kubernetes worker nodes**, numerous types are allowed
- t2.nano, t2.micro, t2.small, t2.medium, t2.large, t2.xlarge, t2.2xlarge
- t3.nano, t3.micro, t3.small, t3.medium, t3.large, t3.xlarge, t3.2xlarge
- m4.large, m4.xlarge, m4.2xlarge, m4.4xlarge, m4.10xlarge
- m5.large, m5.xlarge, m5.2xlarge, m5.4xlarge
...

Recommanded value is mimimum **t2.medium** : 

![15_instancetype k8s worler](https://user-images.githubusercontent.com/58267422/87352139-d89b3400-c55a-11ea-9858-fb07f00fa69e.png)

#### KeyName correspond of name of key we have created previously, choose the name of this key **my-key-gitop**

![16_key_name](https://user-images.githubusercontent.com/58267422/87352518-7262e100-c55b-11ea-992c-28799f927d7b.png)

#### PrivateKey correspond of the private key generated previously **my-key-gitop.pem** 

*Copy and paste the entire private key in the text area* :

![17_privatekey_begin](https://user-images.githubusercontent.com/58267422/87431474-f9a96680-c5e6-11ea-894f-1bf0c898e9b8.png)

![17_privatekey_end](https://user-images.githubusercontent.com/58267422/87352724-d685a500-c55b-11ea-8b91-3f384347e77e.png)


#### SSH location correspond as where from you can connect at the resources EC2, choose **0.0.0.0/0**

![18_ssh_location](https://user-images.githubusercontent.com/58267422/87352935-30866a80-c55c-11ea-84a7-e31326d3d314.png)

**Note : It's not the best option because connection SSH are available from all internet, but it is a lab and not a production environment**

Then click on **Next**

### Configure stack options

- We arrive on the Configure stack options page, In this section, you can leave all default values and click on **Next**.

### Review

- We arrive on the stack review page, In this section, you can view all parameters defined previously to verify it

![19_review parametes](https://user-images.githubusercontent.com/58267422/87353476-2c0e8180-c55d-11ea-989e-090d0c03a29b.png)


- At the bottom of the review page, you must check **Capabilities option** :

- [x] **I acknowledge that AWS CloudFormation might create IAM resources.**

![20_capabilities](https://user-images.githubusercontent.com/58267422/87353737-aa6b2380-c55d-11ea-9e71-7d26d595bc56.png)

**Note : Capabilities means, to allow EC2 instances to perform actions in you AWS Account**
<br> 

- After you review all parameters, read the informative note about capabilities and check the checkbox, you can click on **Create stack** : ![21 create stack](https://user-images.githubusercontent.com/58267422/87353797-cf5f9680-c55d-11ea-84e5-c4d5341e455f.png)


The result must be similar to : 

![22_create_in_progress](https://user-images.githubusercontent.com/58267422/87433147-44c47900-c5e9-11ea-88b7-8d72bd52b831.png)


**Information : Time creating of all resources and environment working can be take 5-10 minutes. You can go take a coffee...** :coffee: :sleeping:
<br>

- After successfully creation  of all resources, the result must be similar to : 

![23_create_complete](https://user-images.githubusercontent.com/58267422/87434111-5c503180-c5ea-11ea-888f-e5cf92384d3a.png)

<br>


## Retrieve information for all resources 

### Get all instance public IP and Private IP addresses 

- In the EC2 section, in the left menu of the window, search **Network & Security > Elastic IPs** and tehn click on **Elastic IPs**:

![24_get_ip_address_resources](https://user-images.githubusercontent.com/58267422/87440961-d7b5e100-c5f2-11ea-9169-7a91bcc1821e.png)


- *Informations about Public IP addresses and Private ip addresses* 

| Resource Name         | Public IP     | Private IP    |
|-----------------------|---------------|---------------|
| EIP GitLabServer      | 3.209.172.186 | 172.31.78.240 |
| EIP KubernetesMaster  | 3.214.7.74    | 172.31.65.133 |
| EIP KubernetesWorker1 | 3.225.242.252 | 172.31.65.155 |
| EIP KubernetesWorker2 | 54.205.78.120 | 172.31.71.244 |

<br>


## SSH connection 

### Connect to EC2 instances using SSH

Now, you can connect to own EC2 resources. You can use your embedded terminal if your using Linux or Mac OS. If you use Windows you can use Putty, MobaXterm...

**Note : Remember for connect to your EC2 instance, it's mandatory to use SSH private key file ```my-key-gitops.pem``` previoulsy generated by AWS**

```
ssh -i "my-key-gitops.pem" admin@3.209.172.186
ssh -i "my-key-gitops.pem" admin@3.214.7.74
ssh -i "my-key-gitops.pem" admin@3.225.242.252
ssh -i "my-key-gitops.pem" admin@54.205.78.120 
```
<br>

## Ensure environment working correctly 

### Ensure GitLab instance is working 

#### Connect to GitLab instance with the following command : 

```
ssh -i "my-key-gitops.pem" admin@3.209.172.186
```

#### Run ```docker ps -a``` to ensure Docker container are Up
```
docker ps -a
```
The result must be similar to : 

```
CONTAINER ID        IMAGE                                COMMAND                  CREATED             STATUS                      PORTS                                                            NAMES
ea6da2d5f127        gitlab/gitlab-runner:alpine-v9.3.0   "/usr/bin/dumb-init …"   5 minutes ago      Up 5 minutes                                                                                gitlab_runner
45a63c7a7012        gitlab/gitlab-ce:11.11.0-ce.0        "/assets/wrapper"        5 minutes ago      Up 5 minutes (healthy)     0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp, 0.0.0.0:2222->22/tcp   gitlab
39889864d5ac        gitlab/gitlab-runner:alpine-v9.3.0   "bash -c 'if [[ -r /…"   5 minutes ago      Exited (0) 2 minutes ago                                                                     gitlab_runner_register
```
**Note : Its normal the gitlab_runner_register container has exited with status code 0**

### Ensure Kubernetes cluster is working

#### Connect to Kubernetes master : 

```
ssh -i "my-key-gitops.pem" admin@3.214.7.74
```

#### Run ```kubectl get nodes``` to ensure all Kubernetes nodes are presents and working as well in the cluster 

```
sudo su -

kubectl get nodes -o wide
```


The result must be similar to : 

```
NAME      STATUS   ROLES    AGE     VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE                       KERNEL-VERSION   CONTAINER-RUNTIME
master    Ready    master   10m   v1.18.5   172.31.65.133   <none>        Debian GNU/Linux 9 (stretch)   4.9.0-8-amd64    docker://19.3.12
worker1   Ready    <none>   9m   v1.18.5   172.31.65.155   <none>        Debian GNU/Linux 9 (stretch)   4.9.0-8-amd64    docker://19.3.12
worker2   Ready    <none>   9m   v1.18.5   172.31.71.244   <none>        Debian GNU/Linux 9 (stretch)   4.9.0-8-amd64    docker://19.3.12
```

#### Create a pod and expose it as a service to ensure network is workin correctly

##### Create a pod nginx :
```
kubectl run nginx --image=nginx --restart=Never

pod/nginx created
```

##### Expose as a service NodePort the pod previoulsly cretaed :  
```
kubectl expose pod nginx --type=NodePort --port=80

service/nginx exposed
```

##### Retrieve informations of resources previously created 
```
kubectl get all
```

The result must be similar to : 

```
NAME        READY   STATUS    RESTARTS   AGE
pod/nginx   1/1     Running   0          12s

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        10h
service/nginx        NodePort    10.107.232.128   <none>        80:31771/TCP   6s
```

##### Test services working correcty on all Kubernetes nodes

###### Get NodePort of nginx service
```
NODE_PORT=$(kubectl get service nginx \
--output=jsonpath='{range .spec.ports[0]}{.nodePort}')
```

###### Get Private IP address of all Kubernetes nodes 
```
KubernetesMasterPrivateIP=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PrivateIpAddress]" \
--filters Name=tag:Name,Values=KubernetesMaster Name=instance-state-name,Values=running \
--output text)

KubernetesWorker1PrivateIP=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PrivateIpAddress]" \
--filters Name=tag:Name,Values=KubernetesWorker1 Name=instance-state-name,Values=running \
--output text)

KubernetesWorker2PrivateIP=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PrivateIpAddress]" \
--filters Name=tag:Name,Values=KubernetesWorker2 Name=instance-state-name,Values=running \
--output text)
```
###### Verify all IP address has been successfully affected to all variables 
```
declare -a array_ip=\
("KubernetesMaster=$KubernetesMasterPrivateIP\n"\
"KubernetesWorker1=$KubernetesWorker1PrivateIP\n""KubernetesWorker2=$KubernetesWorker2PrivateIP")
echo -e ${array_ip[@]}
```

The result must be similar to : 

```
KubernetesMaster=172.31.65.133
KubernetesWorker1=172.31.65.155
KubernetesWorker2=172.31.71.244
```

###### Test the ```nginx``` service can be reached thanks to the routing mesh principle for all Kubernetes nodes


###### From Kubernetes master, ensure the ```nginx``` service can be reached for Kubernetes master node
```
curl $KubernetesMasterPrivateIP:$NODE_PORT
```

###### From Kubernetes master, ensure the ```nginx``` service can be reached for Kubernetes worker 1 node
```
curl $KubernetesWorker1PrivateIP:$NODE_PORT
```
###### From Kubernetes master, ensure the ```nginx``` service can be reached for Kubernetes worker 2 node
```
curl $KubernetesWorker2PrivateIP:$NODE_PORT
```

For all Kubernetes nodes the result must be similar to :

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

### Ensure certficates of GitLab private container registry are present on all Kubernetes cluster nodes

#### Connect to Kubernetes master : 

```
ssh -i "my-key-gitops.pem" admin@3.214.7.74
```

#### Ensure certificate is present

```
GitlabPublicDnsName=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PublicDnsName]" \
--filters Name=tag:Name,Values=GitLabServer Name=instance-state-name,Values=running \
--output text)

ls -l /etc/docker/certs.d/$GitlabPublicDnsName/ca.crt
```
The result must be similar to : 
```
-rw------- 1 admin admin 2065 Jul 13 16:34 /etc/docker/certs.d/ec2-3-209-172-186.compute-1.amazonaws.com/ca.crt
```

#### Connect to Kubernetes worker1 : 

```
ssh -i "my-key-gitops.pem" admin@3.225.242.252
```

#### Ensure certificate is present

```
GitlabPublicDnsName=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PublicDnsName]" \
--filters Name=tag:Name,Values=GitLabServer Name=instance-state-name,Values=running \
--output text)

ls -l /etc/docker/certs.d/$GitlabPublicDnsName/ca.crt
```
The result must be similar to : 
```
-rw------- 1 admin admin 2065 Jul 13 16:34 /etc/docker/certs.d/ec2-3-209-172-186.compute-1.amazonaws.com/ca.crt
```

#### Connect to Kubernetes worker2 : 

```
ssh -i "my-key-gitops.pem" admin@54.205.78.120 
```

#### Ensure certificate is present

```
GitlabPublicDnsName=$(/usr/local/bin/aws ec2 describe-instances \
--query "Reservations[*].Instances[*].[PublicDnsName]" \
--filters Name=tag:Name,Values=GitLabServer Name=instance-state-name,Values=running \
--output text)

ls -l /etc/docker/certs.d/$GitlabPublicDnsName/ca.crt
```
The result must be similar to : 
```
-rw------- 1 admin admin 2065 Jul 13 16:34 /etc/docker/certs.d/ec2-3-209-172-186.compute-1.amazonaws.com/ca.crt
```

## Destroy resources

### Delete AWS CloudFormation stack 

- In the CloudFormation web page select the stack **my-stack-gitops** and then click on **Delete** :

![25_delete_satck](https://user-images.githubusercontent.com/58267422/87361001-92020580-c56b-11ea-95b5-5f0a2fd8a597.png)

- Confirm deletion

![26_confirm_stack_deletion](https://user-images.githubusercontent.com/58267422/87435059-81916f80-c5eb-11ea-8986-e3a714bf8e44.png)

- After clicking on delete, you are notified as the deletion is in progress :

![27_deletion_in_progress](https://user-images.githubusercontent.com/58267422/87361310-35ebb100-c56c-11ea-8b1d-04af2b3ed137.png)

### Delete key pair 

- In the web page **Key Pairs**, in EC2 section, select your key pair **my-key-gitops**, then click on **Actions** and **Delete** :

![28_delet_key_pair](https://user-images.githubusercontent.com/58267422/87361725-24ef6f80-c56d-11ea-82df-14539121bc32.png)

You must type **Delete** in the text area to confirm your deletion and then click on **Delete**

![29_Confirme_key_deletion](https://user-images.githubusercontent.com/58267422/87361931-9cbd9a00-c56d-11ea-86bc-841657caa6fb.png)
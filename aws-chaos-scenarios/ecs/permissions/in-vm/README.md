# Pre-Requisite

Here are the pre-requisites for executing ECS in-vm faults.

## Create Kubernetes Secrets For AWS Auth With HCE

- Make sure the credential used for creating secret should have the permission to perform ECS in-vm faults. [Click Here](./in-vm/permissions.json) for a sample policy file for the fault.
- Ensure the ssm agent is running on the target instances and you have ssm permission as mentioned in [sample policy](./in-vm/permissions.json) to perform in-vm fault.
- Define a Kubernetes secret having the AWS access configuration(key) in the <code>CHAOS_NAMESPACE</code>.
- Make sure to use secret with the <code>default</code> profile.
- A sample secret file looks like:

cloud_secret.yaml

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: cloud-secret
type: Opaque
stringData:
    cloud_config.yml: |-
    # Add the cloud AWS credentials respectively
    [default]
    aws_access_key_id = XXXXXXXXXXXXXXXXXXX
    aws_secret_access_key = XXXXXXXXXXXXXXX
    Aws_session_token = XXXXXXXXXXXXXXX
```

- Create the secret in litmus using:

```bash
kubectl apply -f cloud_secret.yaml -n litmus
```

## Enable Container Metadata

Enable Container Metadata: Ensure that the ECS container metadata is <code>enabled</code>;this feature is <code>disabled</code> by default. Refer AWS docs - <a href="https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-metadata.html">Enabling container metadata</a>. 

<b>NOTE:</b> You need to do the following steps to enable container metadata and attach IAM role to the cluster instances:

- In the EC2 dashboard sidebar click on Launch Configurations under Auto Scaling and create a copy of autoscaling configuration used in target ECS cluster.

- In the new(copied) Launch Configuration update the user data in the launch configuration with <code>ECS_ENABLE_CONTAINER_METADATA</code> to be <code>true</code> as shown below.

<img scr="https://github.com/uditgaurav/chaos-samples/blob/main/aws-chaos-scenarios/img/user-data.png">

- Update the role of the instances in the launch configuration prepared in Step 1

<img src="https://github.com/uditgaurav/chaos-samples/blob/main/aws-chaos-scenarios/img/iam-instance-profile.png">

- Now save the launch configuration by clicking on ‘Create Launch Configuration’.

<img src="https://github.com/uditgaurav/chaos-samples/blob/main/aws-chaos-scenarios/img/create-launch-config.png">

- Update the auto-scaling group to a newer launch configuration

<img src="https://github.com/uditgaurav/chaos-samples/blob/main/aws-chaos-scenarios/img/update-launch-config.png">

- Restart the instances of the ECS cluster to pull the updated configuration:

<img src="https://github.com/uditgaurav/chaos-samples/blob/main/aws-chaos-scenarios/img/restart-instances.png">

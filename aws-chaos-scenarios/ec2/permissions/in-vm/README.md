# Pre-Requisite

Here are the pre-requisites for executing AWS EC2 in-vm chaos faults.

## Create Kubernetes Secrets For AWS Auth With HCE

- Ensure the credential used for creating secret should have the permission to perform EC2 in VM chaos such as stress, network, http and dns chaos. [Click Here]() for a sample policy file for the fault.
- Ensure the ssm agent is running on the target instances and you have ssm permission as mentioned in [sample policy]() to perform in-vm fault.
- Define a Kubernetes secret having the AWS access configuration(key) in the <code>CHAOS_NAMESPACE</code>.
- Ensure to use secret with the <code>default</code> profile.
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

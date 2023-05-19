# Pre-Requisite

Here are the pre-requisites for executing ECS out-of-band faults like instance-stop.

## Create Kubernetes Secrets For AWS Auth With HCE

- Make sure the credential used for creating secret should have the permission to perform ECS out-of-band faults. [Click Here](./permissions.json) for a sample policy file for the fault.
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

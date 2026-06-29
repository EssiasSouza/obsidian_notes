Source: #source/internet_resources 
Project: #project/devops
Areas: #area/work
Subject: #subject/kubernetes
Type: #type/tutorial
Status: #status/learning 
Related: [[Cloud Computing]]
[[Kubernetes Roadmap]]
[[GKE - Google Kubernetes Engine]]

---

# End-to-End Tutorial - Kubernetes Secret Sync to AWS Secrets Manager

This tutorial demonstrates how to:

1. Create a dedicated AWS IAM user
2. Configure restricted permissions
3. Create Kubernetes test secrets
4. Clone the project
5. Configure and execute the script
6. Validate AWS Secrets Manager synchronization
7. Test updates
8. Test key removals

---

# 1. Create an AWS IAM User

Access [AWS IAM Console](https://console.aws.amazon.com/iam/?utm_source=chatgpt.com)

Navigate:

```text id="kt0hpf"
IAM
 └── Users
      └── Create User
```

Create the user:

```text id="jlwmte"
kubernetes-secret-sync
```

Enable:

```text id="ygjlwm"
✔ Programmatic access
```

---

# 2. Create IAM Policy

Navigate:

```text id="b4xdxv"
IAM
 └── Policies
      └── Create Policy
```

Use this policy:

```json id="kuxiws"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListSecrets",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:ListSecrets"
      ],
      "Resource": "*"
    },
    {
      "Sid": "ManageSecrets",
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue",
        "secretsmanager:PutSecretValue",
        "secretsmanager:DescribeSecret",
        "secretsmanager:ListSecretVersionIds",
        "secretsmanager:CreateSecret",
        "secretsmanager:UpdateSecret"
      ],
      "Resource": [
        "arn:aws:secretsmanager:sa-east-1:YOUR_ACCOUNT_ID:secret:production-gke-env*"
      ]
    }
  ]
}
```

Important:

```text id="b3khj4"
production-gke-env
```

must match your Kubernetes cluster prefix.

Example:

```text id="vfxgni"
production-gke-env/ns-secret-test/my-secret
```

Attach the policy to the user.

---

# 3. Configure AWS CLI

Install [AWS CLI](https://aws.amazon.com/cli/?utm_source=chatgpt.com)

Configure credentials:

```bash id="zgw0mn"
aws configure
```

Example:

```text id="l0smmh"
AWS Access Key ID [None]: XXXXXXXXX
AWS Secret Access Key [None]: XXXXXXXXX
Default region name [None]: sa-east-1
Default output format [None]: json
```

Test access:

```bash id="rqddw7"
aws secretsmanager list-secrets
```

---

# 4. Configure kubectl

Install [kubectl](https://kubernetes.io/docs/tasks/tools/?utm_source=chatgpt.com)

Validate Kubernetes access:

```bash id="29z63u"
kubectl get namespaces
```

---

# 5. Create Kubernetes Namespace

Create the namespace:

```bash id="q25qmf"
kubectl create namespace ns-secret-test
```

Validate:

```bash id="xjkh1f"
kubectl get namespaces
```

Expected:

```text id="55q4p7"
ns-secret-test
```

---

# 6. Create Kubernetes Test Secrets

## Secret 1

```bash id="jhnk84"
kubectl create secret generic secret-test-1 \
  -n ns-secret-test \
  --from-literal=username=admin \
  --from-literal=password=123456 \
  --from-literal=environment=production
```

---

## Secret 2

```bash id="r3ndsl"
kubectl create secret generic secret-test-2 \
  -n ns-secret-test \
  --from-literal=api_key=abc123 \
  --from-literal=region=sa-east-1 \
  --from-literal=timeout=30
```

---

## Secret 3

```bash id="9pjrnx"
kubectl create secret generic secret-test-3 \
  -n ns-secret-test \
  --from-literal=db_host=mysql.local \
  --from-literal=db_user=root \
  --from-literal=db_password=secret123
```

---

# 7. Validate Kubernetes Secrets

List secrets:

```bash id="7v0v1x"
kubectl get secrets -n ns-secret-test
```

Expected:

```text id="1q9xdt"
secret-test-1
secret-test-2
secret-test-3
```

---

# 8. Fork the Repository

Open the repository:

[Kubernetes Secret Sync AWS Secret Manager Repository](https://github.com/EssiasSouza/Kubernetes-Secret-Sync-AWS-Secret-Manager.git?utm_source=chatgpt.com)

Click:

```text id="6g76gj"
Fork
```

Then clone:

```bash id="u11k6g"
git clone https://github.com/YOUR_USERNAME/Kubernetes-Secret-Sync-AWS-Secret-Manager.git
```

Enter the directory:

```bash id="o3w3lb"
cd Kubernetes-Secret-Sync-AWS-Secret-Manager
```

---

# 9. Install Python Dependencies

```bash id="1vt3yh"
pip install boto3
```

---

# 10. Configure the Script

Edit:

```text id="ngcz9h"
main_secrets_sync.py
```

Update variables:

```python id="d1mg31"
aws_region = "sa-east-1"
kubernetes_namespace = "ns-secret-test"
prefix = "production-gke-env"
```

---

# 11. Run the Script

Execute:

```bash id="f0b4su"
python main_secrets_sync.py
```

---

# 12. Validate Secrets in AWS Secrets Manager

Access [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/?utm_source=chatgpt.com)

Navigate:

```text id="s2v4zy"
Secrets Manager
 └── Secrets
```

Expected secrets:

```text id="3hsyp8"
production-gke-env/ns-secret-test/secret-test-1
production-gke-env/ns-secret-test/secret-test-2
production-gke-env/ns-secret-test/secret-test-3
```

Open one secret.

Example path:

```text id="9v7p6y"
Secrets Manager
 └── Secrets
      └── production-gke-env/ns-secret-test/secret-test-1
           └── Retrieve secret value
```

Expected JSON:

```json id="csmr5w"
{
  "username": "admin",
  "password": "123456",
  "environment": "production"
}
```

---

# 13. Update Kubernetes Secret Values

Update Secret 1:

```bash id="l0t7yi"
kubectl delete secret secret-test-1 -n ns-secret-test
```

Recreate:

```bash id="f2wd1l"
kubectl create secret generic secret-test-1 \
  -n ns-secret-test \
  --from-literal=username=new-admin \
  --from-literal=password=new-password \
  --from-literal=environment=staging
```

---

Update Secret 2:

```bash id="2gjlwm"
kubectl delete secret secret-test-2 -n ns-secret-test
```

```bash id="3dzqcw"
kubectl create secret generic secret-test-2 \
  -n ns-secret-test \
  --from-literal=api_key=xyz999 \
  --from-literal=region=us-east-1 \
  --from-literal=timeout=60
```

---

Update Secret 3:

```bash id="jlwmhc"
kubectl delete secret secret-test-3 -n ns-secret-test
```

```bash id="tx8bho"
kubectl create secret generic secret-test-3 \
  -n ns-secret-test \
  --from-literal=db_host=postgres.local \
  --from-literal=db_user=postgres \
  --from-literal=db_password=supersecret
```

---

# 14. Run the Script Again

```bash id="5jlvh2"
python main_secrets_sync.py
```

---

# 15. Validate Updated Secrets

Navigate again:

```text id="9vl5m2"
Secrets Manager
 └── Secrets
```

Open:

```text id="h2xuzt"
production-gke-env/ns-secret-test/secret-test-1
```

Expected:

```json id="kg9d0i"
{
  "username": "new-admin",
  "password": "new-password",
  "environment": "staging"
}
```

---

# 16. Remove One Key From Each Secret

## Secret 1

Remove:

```text id="wy7sd8"
environment
```

```bash id="ruxb7y"
kubectl delete secret secret-test-1 -n ns-secret-test
```

```bash id="g8sxg6"
kubectl create secret generic secret-test-1 \
  -n ns-secret-test \
  --from-literal=username=new-admin \
  --from-literal=password=new-password
```

---

## Secret 2

Remove:

```text id="0l7u2q"
timeout
```

```bash id="nlqeq4"
kubectl delete secret secret-test-2 -n ns-secret-test
```

```bash id="9vjlwm"
kubectl create secret generic secret-test-2 \
  -n ns-secret-test \
  --from-literal=api_key=xyz999 \
  --from-literal=region=us-east-1
```

---

## Secret 3

Remove:

```text id="3mlt4s"
db_password
```

```bash id="7cy0jm"
kubectl delete secret secret-test-3 -n ns-secret-test
```

```bash id="jlwm2m"
kubectl create secret generic secret-test-3 \
  -n ns-secret-test \
  --from-literal=db_host=postgres.local \
  --from-literal=db_user=postgres
```

---

# 17. Run the Script Again

```bash id="g4qn3u"
python main_secrets_sync.py
```

---

# 18. Validate Final Results

Open AWS Secrets Manager again:

```text id="mtgxwx"
Secrets Manager
 └── Secrets
```

Important behavior:

The current script:

* updates existing keys
* adds new keys
* DOES NOT remove old AWS keys automatically

Example:

If AWS already contains:

```json id="5e1d73"
{
  "username": "new-admin",
  "password": "new-password",
  "environment": "staging"
}
```

and Kubernetes now contains:

```json id="jlwmyn"
{
  "username": "new-admin",
  "password": "new-password"
}
```

AWS will remain:

```json id="jlwm2f"
{
  "username": "new-admin",
  "password": "new-password",
  "environment": "staging"
}
```

because the script currently preserves removed keys.

---

# 19. Review Generated Logs

The script generates logs like:

```text id="uxgr7x"
secret_sync_20260519_210000.log
```

Example:

```
2026-05-19 21:00:00,100 | INFO | Updating on AWS: cluster-x/ns-secret/secret-test-1
2026-05-19 21:00:00,400 | INFO | [UPDATED] cluster-x/ns-secret/secret-test-1
```

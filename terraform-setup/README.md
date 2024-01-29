# kubernetes-with-linode
## Terraform Setup 
- [Install Terraform](/terraform-setup/terrafom-install.sh)
## Kubectl Setup
- [Install Kubectl ](/kubectl-install.sh)

## 1.Clone this repo
```bash
git clone https://github.com/aungkohtat/kubernetes-with-linode.git
```

cd kubernetes-with-linode

## 2. Add Linode Token
```bash
echo "linode_api_token=\"YOUR_API_KEY\"" >> terraform.tfvars
```

## 3. Initialize Terraform
```bash
 Initialize Terraform
```
```bash
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/local from the dependency lock file
- Reusing previous version of linode/linode from the dependency lock file
- Using previously-installed linode/linode v2.13.0
- Using previously-installed hashicorp/local v2.4.1

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ 
```

## 4. Terraform Plan
- `terraform plan`

```bash
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # linode_lke_cluster.terraform_k8s will be created
  + resource "linode_lke_cluster" "terraform_k8s" {
      + api_endpoints = (known after apply)
      + dashboard_url = (known after apply)
      + id            = (known after apply)
      + k8s_version   = "1.25"
      + kubeconfig    = (sensitive value)
      + label         = "terraform-k8s"
      + region        = "us-east"
      + status        = (known after apply)
      + tags          = [
          + "terraform-k8s",
        ]

      + pool {
          + count = 3
          + id    = (known after apply)
          + nodes = (known after apply)
          + type  = "g6-standard-1"
        }
    }

  # local_file.k8s_config will be created
  + resource "local_file" "k8s_config" {
      + content              = (known after apply)
      + content_base64sha256 = (known after apply)
      + content_base64sha512 = (known after apply)
      + content_md5          = (known after apply)
      + content_sha1         = (known after apply)
      + content_sha256       = (known after apply)
      + content_sha512       = (known after apply)
      + directory_permission = "0777"
      + file_permission      = "0600"
      + filename             = "/workspace/kubernetes-with-linode/terraform-setup/.kube/kubeconfig.yaml"
      + id                   = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ 
```
## 5. Check Terraform Validete
- `terraform validate`

```bash
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ terraform validate
Success! The configuration is valid.

gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ 
```


## 6. Terraform your Kubernetes Cluster
```bash
terraform apply
```
## 7. Set your KUBECONFIG Environment Variable

```bash
export KUBECONFIG="./.kube/kubeconfig.yaml"
```

## 8. Check Kubernetes Cluster Nodes
```bash
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ kubectl get nodes
NAME                            STATUS   ROLES    AGE     VERSION
lke153384-224476-47af680d0000   Ready    <none>   6m42s   v1.28.3
lke153384-224476-533bbba10000   Ready    <none>   6m48s   v1.28.3
lke153384-224476-59a2b8e10000   Ready    <none>   6m44s   v1.28.3
```
## 9. Deploy your first app
-`kubectl apply -f k8s.yaml`

```bash
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ kubectl apply -f k8s.yaml
deployment.apps/cfe-nginx-deployment created
service/cfe-nginx-service created
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
cfe-nginx-deployment-78b5c49779-d5nps   1/1     Running   0          8s
cfe-nginx-deployment-78b5c49779-r94g6   1/1     Running   0          8s
cfe-nginx-deployment-78b5c49779-ztvnx   1/1     Running   0          8s
gitpod /workspace/kubernetes-with-linode/terraform-setup (main) $ 
```

## 10. Clean-up
- `terraform apply -destroy -auto-approve`

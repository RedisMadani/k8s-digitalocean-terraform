# Kubernetes Cluster Deployment on DigitalOcean with Terraform

This guide will help you deploy a Kubernetes cluster on DigitalOcean using Terraform, utilizing kubeadm for Kubernetes component installation. This setup also includes Ubuntu as the operating system and includes an ingress controller by default.

## Prerequisites

Before getting started, ensure you have the following prerequisites:

- A DigitalOcean account
- DigitalOcean Token (Generate one in DO's settings/tokens)
- Terraform installed
- CloudFlare's PKI/TLS toolkit cfssl (On Mac, install with `brew install terraform cfssl kubectl`)

## Step-by-Step Deployment

Follow these steps to deploy your Kubernetes cluster:

**1. Generate SSH Key:**

Generate an SSH key pair by running the following command and saving it as `~/.ssh/id_rsa`:

```
ssh-keygen -t rsa -b 4096
```

**2. Add SSH Key to DigitalOcean:**

Add your public key to the DigitalOcean control panel. Provide a name for the key and paste the public key there.

**3. Set up SSH Agent:**

Run the following commands to set up your SSH agent and add the SSH key:

```bash
eval `ssh-agent -s`
ssh-add ~/.ssh/id_rsa
```

**4. Clone the Repository:**

Clone this repository to your local machine or download it.

**5. Set Environment Variables:**

Navigate to the repository directory and set environment variables with the following commands:

```bash
export TF_VAR_number_of_workers=3
export TF_VAR_do_token=$(cat ./secrets/DO_TOKEN)
export TF_VAR_ssh_fingerprint=$(ssh-keygen -E MD5 -lf ~/.ssh/id_rsa.pub | awk '{print $2}' | sed 's/MD5://g')
```

If you are using an older version of OpenSSH (<6.9), replace the last line with:

```bash
export TF_VAR_ssh_fingerprint=$(ssh-keygen -lf ~/.ssh/id_rsa.pub | awk '{print $2}')
```

**6. Optionally Set Datacenter Region (Default is nyc3):**

You can specify a different datacenter region by running:

```bash
export TF_VAR_do_region=fra1
```

Choose the region from the available DigitalOcean regions.

**7. Set up Terraform:**

Execute the following command to set up Terraform:

```bash
./setup_terraform.sh
```

**8. Deploy Kubernetes Cluster:**

Deploy the Kubernetes cluster by running:

```bash
terraform apply
```

**9. Configure kubectl:**

Set up `kubectl` to use the cluster configuration file:

```bash
export KUBECONFIG=/path/to/your/k8s-digitalocean-terraform/secrets/admin.conf
```

**10. Verify Cluster:**

Verify that your cluster is up and running by checking the nodes:

```bash
kubectl get nodes
```

You should see the list of nodes in your Kubernetes cluster.

## Congratulations!

You have successfully deployed a Kubernetes cluster on DigitalOcean using Terraform, with Ubuntu as the operating system and an ingress controller included.

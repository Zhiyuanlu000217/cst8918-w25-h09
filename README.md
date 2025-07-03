# AKS Cluster Terraform Configuration

This Terraform configuration creates an Azure Kubernetes Service (AKS) cluster with the following specifications:

## Cluster Specifications

- **Minimum nodes**: 1
- **Maximum nodes**: 3
- **VM size**: Standard_B2s
- **Kubernetes version**: 1.28.5 (latest stable)
- **Identity**: SystemAssigned managed identity
- **Resource Group**: New resource group created specifically for this cluster
- **Location**: East US

## Prerequisites

1. **Azure CLI** installed and authenticated
2. **Terraform** installed (version >= 1.0)
3. **kubectl** installed

## Files

- `main.tf` - Main Terraform configuration
- `variables.tf` - Variable definitions
- `terraform.tfvars` - Variable values
- `sample-app.yaml` - Sample Kubernetes application to deploy

## Usage

### 1. Initialize Terraform

```bash
terraform init
```

### 2. Plan the deployment

```bash
terraform plan
```

### 3. Apply the configuration

```bash
terraform apply
```

### 4. Get the kubeconfig and connect to the cluster

After successful deployment, run the following commands to connect to your AKS cluster:

```bash
# Save the kubeconfig to a file
echo "$(terraform output kube_config)" > ./kubeconfig

# Set the KUBECONFIG environment variable
export KUBECONFIG=./kubeconfig

# Verify connection to the cluster
kubectl cluster-info
kubectl get nodes
```

### 5. Deploy the sample application

```bash
kubectl apply -f sample-app.yaml
```

### 6. Check the deployment

```bash
kubectl get pods
kubectl get services
```

## Outputs

The Terraform configuration provides the following outputs:

- `kube_config` - The kubeconfig file content (sensitive)
- `cluster_name` - Name of the created AKS cluster
- `resource_group_name` - Name of the resource group
- `cluster_endpoint` - The cluster API endpoint

## Cleanup

To destroy all created resources:

```bash
terraform destroy
```

## Notes

- The cluster uses auto-scaling with a minimum of 1 node and maximum of 3 nodes
- The cluster is created in a new resource group named `cst8918-lab9-rg`
- The cluster uses the latest stable Kubernetes version (1.28.5)
- All resources are tagged appropriately for cost tracking
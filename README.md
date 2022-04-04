You must use either Terraform, AWS CloudFormation or AWS CDK for all of the following tasks.

Create code for deploying a VPC in AWS with 2 public and 2 private subnets.

Create code for deploying an EKS cluster in AWS, which will use the VPC created in the previous step. The cluster must have 2 nodes, using instance type t3a.large. The nodes must be on the private subnets only.

Add a README.md to the root directory of your project, with instructions for the team to deploy the infrastructure you created.

Publish your code to a public Git repository in a platform of your choice (e.g. GitHub, GitLab, Bitbucket, etc.), so that it can be cloned and tested by the team.


+++Instruction+++

2 vpc has been created , EKS will be created and with the desired instance type .

First you need to make a terraform plan after terraform init and then apply it.

```json

terraform init

terraform plan

terraform apply 
'''

As you can see below, the output of the plan is as follows

P.S. tf file must be in specific folder

-------------------------------------------------------------------------------------------------------------------
```bash


ubuntu@bastion-node:~/terraform_test$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

module.eks.data.aws_ami.eks_worker_windows: Refreshing state...
module.eks.data.aws_caller_identity.current: Refreshing state...
module.eks.data.aws_iam_policy_document.cluster_assume_role_policy: Refreshing state...
data.aws_availability_zones.available: Refreshing state...
module.eks.data.aws_iam_policy_document.cluster_elb_sl_role_creation[0]: Refreshing state...
module.eks.data.aws_ami.eks_worker: Refreshing state...
module.eks.data.aws_partition.current: Refreshing state...
module.eks.data.aws_iam_policy_document.workers_assume_role_policy: Refreshing state...

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
 <= read (data resources)

Terraform will perform the following actions:

  # data.aws_eks_cluster.cluster will be read during apply
  # (config refers to values not yet known)
 <= data "aws_eks_cluster" "cluster"  {
      + arn                       = (known after apply)
      + certificate_authority     = (known after apply)
      + created_at                = (known after apply)
      + enabled_cluster_log_types = (known after apply)
      + endpoint                  = (known after apply)
      + id                        = (known after apply)
      + identity                  = (known after apply)
      + kubernetes_network_config = (known after apply)
      + name                      = (known after apply)
      + platform_version          = (known after apply)
      + role_arn                  = (known after apply)
      + status                    = (known after apply)
      + tags                      = (known after apply)
      + version                   = (known after apply)
      + vpc_config                = (known after apply)
    }

  # data.aws_eks_cluster_auth.cluster will be read during apply
  # (config refers to values not yet known)
 <= data "aws_eks_cluster_auth" "cluster"  {
      + id    = (known after apply)
      + name  = (known after apply)
      + token = (sensitive value)
    }

  # module.eks.aws_eks_cluster.this[0] will be created
  + resource "aws_eks_cluster" "this" {
      + arn                   = (known after apply)
      + certificate_authority = (known after apply)
      + created_at            = (known after apply)
      + endpoint              = (known after apply)
      + id                    = (known after apply)
      + identity              = (known after apply)
      + name                  = "eks-cluster"
      + platform_version      = (known after apply)
      + role_arn              = (known after apply)
      + status                = (known after apply)
      + version               = "1.17"

      + kubernetes_network_config {
          + service_ipv4_cidr = (known after apply)
        }

      + timeouts {
          + create = "30m"
          + delete = "15m"
        }

      + vpc_config {
          + cluster_security_group_id = (known after apply)
          + endpoint_private_access   = false
          + endpoint_public_access    = true
          + public_access_cidrs       = [
              + "0.0.0.0/0",
            ]
          + security_group_ids        = (known after apply)
          + subnet_ids                = (known after apply)
          + vpc_id                    = (known after apply)
        }
    }

  # module.eks.aws_iam_role.cluster[0] will be created
  + resource "aws_iam_role" "cluster" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = "sts:AssumeRole"
                      + Effect    = "Allow"
                      + Principal = {
                          + Service = "eks.amazonaws.com"
                        }
                      + Sid       = "EKSClusterAssumeRole"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + create_date           = (known after apply)
      + force_detach_policies = true
      + id                    = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = (known after apply)
      + name_prefix           = "eks-cluster"
      + path                  = "/"
      + unique_id             = (known after apply)

      + inline_policy {
          + name   = (known after apply)
          + policy = (known after apply)
        }
    }

  # module.eks.aws_iam_role.workers[0] will be created
  + resource "aws_iam_role" "workers" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = "sts:AssumeRole"
                      + Effect    = "Allow"
                      + Principal = {
                          + Service = "ec2.amazonaws.com"
                        }
                      + Sid       = "EKSWorkerAssumeRole"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + create_date           = (known after apply)
      + force_detach_policies = true
      + id                    = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = (known after apply)
      + name_prefix           = "eks-cluster"
      + path                  = "/"
      + unique_id             = (known after apply)

      + inline_policy {
          + name   = (known after apply)
          + policy = (known after apply)
        }
    }

  # module.eks.aws_iam_role_policy.cluster_elb_sl_role_creation[0] will be created
  + resource "aws_iam_role_policy" "cluster_elb_sl_role_creation" {
      + id          = (known after apply)
      + name        = (known after apply)
      + name_prefix = "eks-cluster-elb-sl-role-creation"
      + policy      = jsonencode(
            {
              + Statement = [
                  + {
                      + Action   = [
                          + "ec2:DescribeInternetGateways",
                          + "ec2:DescribeAccountAttributes",
                        ]
                      + Effect   = "Allow"
                      + Resource = "*"
                      + Sid      = ""
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + role        = (known after apply)
    }

  # module.eks.aws_iam_role_policy_attachment.cluster_AmazonEKSClusterPolicy[0] will be created
  + resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSClusterPolicy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"
      + role       = (known after apply)
    }

  # module.eks.aws_iam_role_policy_attachment.cluster_AmazonEKSServicePolicy[0] will be created
  + resource "aws_iam_role_policy_attachment" "cluster_AmazonEKSServicePolicy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSServicePolicy"
      + role       = (known after apply)
    }

  # module.eks.aws_iam_role_policy_attachment.workers_AmazonEC2ContainerRegistryReadOnly[0] will be created
  + resource "aws_iam_role_policy_attachment" "workers_AmazonEC2ContainerRegistryReadOnly" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"
      + role       = (known after apply)
    }

  # module.eks.aws_iam_role_policy_attachment.workers_AmazonEKSWorkerNodePolicy[0] will be created
  + resource "aws_iam_role_policy_attachment" "workers_AmazonEKSWorkerNodePolicy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"
      + role       = (known after apply)
    }

  # module.eks.aws_iam_role_policy_attachment.workers_AmazonEKS_CNI_Policy[0] will be created
  + resource "aws_iam_role_policy_attachment" "workers_AmazonEKS_CNI_Policy" {
      + id         = (known after apply)
      + policy_arn = "arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"
      + role       = (known after apply)
    }

  # module.eks.aws_security_group.cluster[0] will be created
  + resource "aws_security_group" "cluster" {
      + arn                    = (known after apply)
      + description            = "EKS cluster security group."
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = (known after apply)
      + name                   = (known after apply)
      + name_prefix            = "eks-cluster"
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags                   = {
          + "Name" = "eks-cluster-eks_cluster_sg"
        }
      + vpc_id                 = (known after apply)
    }

  # module.eks.aws_security_group.workers[0] will be created
  + resource "aws_security_group" "workers" {
      + arn                    = (known after apply)
      + description            = "Security group for all nodes in the cluster."
      + egress                 = (known after apply)
      + id                     = (known after apply)
      + ingress                = (known after apply)
      + name                   = (known after apply)
      + name_prefix            = "eks-cluster"
      + owner_id               = (known after apply)
      + revoke_rules_on_delete = false
      + tags                   = {
          + "Name"                              = "eks-cluster-eks_worker_sg"
          + "kubernetes.io/cluster/eks-cluster" = "owned"
        }
      + vpc_id                 = (known after apply)
    }

  # module.eks.aws_security_group_rule.cluster_egress_internet[0] will be created
  + resource "aws_security_group_rule" "cluster_egress_internet" {
      + cidr_blocks              = [
          + "0.0.0.0/0",
        ]
      + description              = "Allow cluster egress access to the Internet."
      + from_port                = 0
      + id                       = (known after apply)
      + protocol                 = "-1"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 0
      + type                     = "egress"
    }

  # module.eks.aws_security_group_rule.cluster_https_worker_ingress[0] will be created
  + resource "aws_security_group_rule" "cluster_https_worker_ingress" {
      + description              = "Allow pods to communicate with the EKS cluster API."
      + from_port                = 443
      + id                       = (known after apply)
      + protocol                 = "tcp"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 443
      + type                     = "ingress"
    }

  # module.eks.aws_security_group_rule.workers_egress_internet[0] will be created
  + resource "aws_security_group_rule" "workers_egress_internet" {
      + cidr_blocks              = [
          + "0.0.0.0/0",
        ]
      + description              = "Allow nodes all egress to the Internet."
      + from_port                = 0
      + id                       = (known after apply)
      + protocol                 = "-1"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 0
      + type                     = "egress"
    }

  # module.eks.aws_security_group_rule.workers_ingress_cluster[0] will be created
  + resource "aws_security_group_rule" "workers_ingress_cluster" {
      + description              = "Allow workers pods to receive communication from the cluster control plane."
      + from_port                = 1025
      + id                       = (known after apply)
      + protocol                 = "tcp"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 65535
      + type                     = "ingress"
    }

  # module.eks.aws_security_group_rule.workers_ingress_cluster_https[0] will be created
  + resource "aws_security_group_rule" "workers_ingress_cluster_https" {
      + description              = "Allow pods running extension API servers on port 443 to receive communication from cluster control plane."
      + from_port                = 443
      + id                       = (known after apply)
      + protocol                 = "tcp"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 443
      + type                     = "ingress"
    }

  # module.eks.aws_security_group_rule.workers_ingress_self[0] will be created
  + resource "aws_security_group_rule" "workers_ingress_self" {
      + description              = "Allow node to communicate with each other."
      + from_port                = 0
      + id                       = (known after apply)
      + protocol                 = "-1"
      + security_group_id        = (known after apply)
      + self                     = false
      + source_security_group_id = (known after apply)
      + to_port                  = 65535
      + type                     = "ingress"
    }

  # module.eks.kubernetes_config_map.aws_auth[0] will be created
  + resource "kubernetes_config_map" "aws_auth" {
      + data = (known after apply)
      + id   = (known after apply)

      + metadata {
          + generation       = (known after apply)
          + name             = "aws-auth"
          + namespace        = "kube-system"
          + resource_version = (known after apply)
          + self_link        = (known after apply)
          + uid              = (known after apply)
        }
    }

  # module.eks.local_file.kubeconfig[0] will be created
  + resource "local_file" "kubeconfig" {
      + content              = (known after apply)
      + directory_permission = "0755"
      + file_permission      = "0644"
      + filename             = "./kubeconfig_eks-cluster"
      + id                   = (known after apply)
    }

  # module.eks.null_resource.wait_for_cluster[0] will be created
  + resource "null_resource" "wait_for_cluster" {
      + id = (known after apply)
    }

  # module.vpc.aws_eip.nat[0] will be created
  + resource "aws_eip" "nat" {
      + allocation_id        = (known after apply)
      + association_id       = (known after apply)
      + carrier_ip           = (known after apply)
      + customer_owned_ip    = (known after apply)
      + domain               = (known after apply)
      + id                   = (known after apply)
      + instance             = (known after apply)
      + network_border_group = (known after apply)
      + network_interface    = (known after apply)
      + private_dns          = (known after apply)
      + private_ip           = (known after apply)
      + public_dns           = (known after apply)
      + public_ip            = (known after apply)
      + public_ipv4_pool     = (known after apply)
      + tags                 = {
          + "Name" = "k8s-vpc-ap-south-1a"
        }
      + vpc                  = true
    }

  # module.vpc.aws_internet_gateway.this[0] will be created
  + resource "aws_internet_gateway" "this" {
      + arn      = (known after apply)
      + id       = (known after apply)
      + owner_id = (known after apply)
      + tags     = {
          + "Name" = "k8s-vpc"
        }
      + vpc_id   = (known after apply)
    }

  # module.vpc.aws_nat_gateway.this[0] will be created
  + resource "aws_nat_gateway" "this" {
      + allocation_id        = (known after apply)
      + id                   = (known after apply)
      + network_interface_id = (known after apply)
      + private_ip           = (known after apply)
      + public_ip            = (known after apply)
      + subnet_id            = (known after apply)
      + tags                 = {
          + "Name" = "k8s-vpc-ap-south-1a"
        }
    }

  # module.vpc.aws_route.private_nat_gateway[0] will be created
  + resource "aws_route" "private_nat_gateway" {
      + destination_cidr_block = "0.0.0.0/0"
      + id                     = (known after apply)
      + instance_id            = (known after apply)
      + instance_owner_id      = (known after apply)
      + nat_gateway_id         = (known after apply)
      + network_interface_id   = (known after apply)
      + origin                 = (known after apply)
      + route_table_id         = (known after apply)
      + state                  = (known after apply)

      + timeouts {
          + create = "5m"
        }
    }

  # module.vpc.aws_route.public_internet_gateway[0] will be created
  + resource "aws_route" "public_internet_gateway" {
      + destination_cidr_block = "0.0.0.0/0"
      + gateway_id             = (known after apply)
      + id                     = (known after apply)
      + instance_id            = (known after apply)
      + instance_owner_id      = (known after apply)
      + network_interface_id   = (known after apply)
      + origin                 = (known after apply)
      + route_table_id         = (known after apply)
      + state                  = (known after apply)

      + timeouts {
          + create = "5m"
        }
    }

  # module.vpc.aws_route_table.private[0] will be created
  + resource "aws_route_table" "private" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + owner_id         = (known after apply)
      + propagating_vgws = (known after apply)
      + route            = (known after apply)
      + tags             = {
          + "Name" = "k8s-vpc-private"
        }
      + vpc_id           = (known after apply)
    }

  # module.vpc.aws_route_table.public[0] will be created
  + resource "aws_route_table" "public" {
      + arn              = (known after apply)
      + id               = (known after apply)
      + owner_id         = (known after apply)
      + propagating_vgws = (known after apply)
      + route            = (known after apply)
      + tags             = {
          + "Name" = "k8s-vpc-public"
        }
      + vpc_id           = (known after apply)
    }

  # module.vpc.aws_route_table_association.private[0] will be created
  + resource "aws_route_table_association" "private" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # module.vpc.aws_route_table_association.private[1] will be created
  + resource "aws_route_table_association" "private" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # module.vpc.aws_route_table_association.public[0] will be created
  + resource "aws_route_table_association" "public" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # module.vpc.aws_route_table_association.public[1] will be created
  + resource "aws_route_table_association" "public" {
      + id             = (known after apply)
      + route_table_id = (known after apply)
      + subnet_id      = (known after apply)
    }

  # module.vpc.aws_subnet.private[0] will be created
  + resource "aws_subnet" "private" {
      + arn                             = (known after apply)
      + assign_ipv6_address_on_creation = false
      + availability_zone               = "ap-south-1a"
      + availability_zone_id            = (known after apply)
      + cidr_block                      = "172.16.1.0/24"
      + id                              = (known after apply)
      + ipv6_cidr_block_association_id  = (known after apply)
      + map_public_ip_on_launch         = false
      + owner_id                        = (known after apply)
      + tags                            = {
          + "Name"                              = "k8s-vpc-private-ap-south-1a"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/internal-elb"   = "1"
        }
      + tags_all                        = {
          + "Name"                              = "k8s-vpc-private-ap-south-1a"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/internal-elb"   = "1"
        }
      + vpc_id                          = (known after apply)
    }

  # module.vpc.aws_subnet.private[1] will be created
  + resource "aws_subnet" "private" {
      + arn                             = (known after apply)
      + assign_ipv6_address_on_creation = false
      + availability_zone               = "ap-south-1b"
      + availability_zone_id            = (known after apply)
      + cidr_block                      = "172.16.2.0/24"
      + id                              = (known after apply)
      + ipv6_cidr_block_association_id  = (known after apply)
      + map_public_ip_on_launch         = false
      + owner_id                        = (known after apply)
      + tags                            = {
          + "Name"                              = "k8s-vpc-private-ap-south-1b"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/internal-elb"   = "1"
        }
      + tags_all                        = {
          + "Name"                              = "k8s-vpc-private-ap-south-1b"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/internal-elb"   = "1"
        }
      + vpc_id                          = (known after apply)
    }

  # module.vpc.aws_subnet.public[0] will be created
  + resource "aws_subnet" "public" {
      + arn                             = (known after apply)
      + assign_ipv6_address_on_creation = false
      + availability_zone               = "ap-south-1a"
      + availability_zone_id            = (known after apply)
      + cidr_block                      = "172.16.4.0/24"
      + id                              = (known after apply)
      + ipv6_cidr_block_association_id  = (known after apply)
      + map_public_ip_on_launch         = true
      + owner_id                        = (known after apply)
      + tags                            = {
          + "Name"                              = "k8s-vpc-public-ap-south-1a"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/elb"            = "1"
        }
      + tags_all                        = {
          + "Name"                              = "k8s-vpc-public-ap-south-1a"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/elb"            = "1"
        }
      + vpc_id                          = (known after apply)
    }

  # module.vpc.aws_subnet.public[1] will be created
  + resource "aws_subnet" "public" {
      + arn                             = (known after apply)
      + assign_ipv6_address_on_creation = false
      + availability_zone               = "ap-south-1b"
      + availability_zone_id            = (known after apply)
      + cidr_block                      = "172.16.5.0/24"
      + id                              = (known after apply)
      + ipv6_cidr_block_association_id  = (known after apply)
      + map_public_ip_on_launch         = true
      + owner_id                        = (known after apply)
      + tags                            = {
          + "Name"                              = "k8s-vpc-public-ap-south-1b"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/elb"            = "1"
        }
      + tags_all                        = {
          + "Name"                              = "k8s-vpc-public-ap-south-1b"
          + "kubernetes.io/cluster/eks-cluster" = "shared"
          + "kubernetes.io/role/elb"            = "1"
        }
      + vpc_id                          = (known after apply)
    }

  # module.vpc.aws_vpc.this[0] will be created
  + resource "aws_vpc" "this" {
      + arn                              = (known after apply)
      + assign_generated_ipv6_cidr_block = false
      + cidr_block                       = "172.16.0.0/16"
      + default_network_acl_id           = (known after apply)
      + default_route_table_id           = (known after apply)
      + default_security_group_id        = (known after apply)
      + dhcp_options_id                  = (known after apply)
      + enable_classiclink               = (known after apply)
      + enable_classiclink_dns_support   = (known after apply)
      + enable_dns_hostnames             = true
      + enable_dns_support               = true
      + id                               = (known after apply)
      + instance_tenancy                 = "default"
      + ipv6_association_id              = (known after apply)
      + ipv6_cidr_block                  = (known after apply)
      + main_route_table_id              = (known after apply)
      + owner_id                         = (known after apply)
      + tags                             = {
          + "Name" = "k8s-vpc"
        }
      + tags_all                         = {
          + "Name" = "k8s-vpc"
        }
    }

  # module.eks.module.node_groups.aws_eks_node_group.workers["first"] will be created
  + resource "aws_eks_node_group" "workers" {
      + ami_type        = (known after apply)
      + arn             = (known after apply)
      + capacity_type   = (known after apply)
      + cluster_name    = "eks-cluster"
      + disk_size       = (known after apply)
      + id              = (known after apply)
      + instance_types  = (known after apply)
      + node_group_name = (known after apply)
      + node_role_arn   = (known after apply)
      + release_version = (known after apply)
      + resources       = (known after apply)
      + status          = (known after apply)
      + subnet_ids      = (known after apply)
      + version         = (known after apply)

      + remote_access {
          + ec2_ssh_key               = (known after apply)
          + source_security_group_ids = (known after apply)
        }

      + scaling_config {
          + desired_size = (known after apply)
          + max_size     = (known after apply)
          + min_size     = (known after apply)
        }
    }

  # module.eks.module.node_groups.random_pet.node_groups["first"] will be created
  + resource "random_pet" "node_groups" {
      + id        = (known after apply)
      + keepers   = (known after apply)
      + length    = 2
      + separator = "-"
    }

Plan: 38 to add, 0 to change, 0 to destroy.# terraform
```

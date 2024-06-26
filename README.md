vpc terraform module
===========

## This repository will not have active support any more.

---

A terraform module to provide a VPC in AWS.


Module Input Variables
----------------------

- `name` - name to be used on all the resources created by the module
- `cidr` - the CIDR block for the VPC
- `instance_tenancy` - tenancy option for instances launched into the VPC
- `public_subnets` - list of public subnet cidrs
- `private_subnets` - list of private subnet cidrs
- `database_subnets` - list of private RDS subnet cidrs
- `create_database_subnet_group` - Controls, if should database subnet group be created
- `elasticache_subnets` - list of private Elasticache subnet cidrs
- `azs` - list of AZs in which to distribute subnets
- `enable_dns_hostnames` - should be true if you want to use private DNS within the VPC
- `enable_dns_support` - should be true if you want to use private DNS within the VPC
- `enable_classiclink` - should be true if you want to use ClassicLink within the VPC
- `enable_classiclink_dns_support` - should be true if you want to use private DNS within the classiclinks
- `enable_nat_gateway` - should be true if you want to provision NAT Gateways
- `single_nat_gateway` - should be true if you want to provision a single shared NAT Gateway across all of your private networks
- `enable_s3_endpoint` - should be true if you want to provision an S3 endpoint within the VPC
- `enable_dynamodb_endpoint` - should be true if you want to provision a DynamoDB endpoint within the VPC
- `map_public_ip_on_launch` - should be false if you do not want to auto-assign public IP on launch
- `private_propagating_vgws` - list of VGWs the private route table should propagate
- `public_propagating_vgws` - list of VGWs the public route table should propagate
- `tags` - dictionary of tags that will be added to resources created by the module
- `public_subnet_tags` - dictionary of tags that will be added to public subnets created by the module
- `private_subnet_tags` - dictionary of tags that will be added to private subnets created by the module
- `database_subnet_tags` - dictionary of tags that will be added to database subnets created by the module
- `elasticache_subnet_tags` - dictionary of tags that will be added to elasticache subnets created by the module

It's generally preferable to keep `public_subnets`, `private_subnets`, and
`azs` to lists of the same length.

This module optionally creates NAT Gateways (one per availability zone) and sets them
as the default gateways for the corresponding private subnets.

Usage
-----

```hcl
module "vpc" {
  source = "Repo Source"

  name = "my-vpc"

  cidr = "10.0.0.0/16"
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway = "true"

  azs      = ["us-west-2a", "us-west-2b", "us-west-2c"]

  tags {
    "Terraform" = "true"
    "Environment" = "${var.environment}"
  }
}
```

Outputs
=======

 - `vpc_id` - does what it says on the tin
 - `private_subnets` - list of private subnet ids
 - `public_subnets` - list of public subnet ids
 - `database_subnets` - list of database subnets ids
 - `database_subnet_group` - db subnet group name
 - `elasticache_subnets` - list of elasticache subnets ids
 - `elasticache_subnet_group` - elasticache subnet group name
 - `public_route_table_ids` - list of public route table ids
 - `private_route_table_ids` - list of private route table ids
 - `default_security_group_id` - VPC default security group id string
 - `nat_eips` - list of Elastic IP ids (if any are provisioned)
 - `nat_eips_public_ips` - list of NAT gateways' public Elastic IP's (if any are provisioned)
 - `natgw_ids` - list of NAT gateway ids
 - `igw_id` - Internet Gateway id string
 - `default_network_acl_id` - VPC default network ACL id
 - `vpc_endpoint_s3_id` - VPC Endpoint ID for S3
 - `vpc_endpoint_dynamodb_id` - VPC Endpoint ID for Dynamodb

**NOTE**: previous versions of this module returned a single string as a route
table ID, while this version returns a list.


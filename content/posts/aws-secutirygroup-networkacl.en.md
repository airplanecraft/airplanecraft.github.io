+++
Categories = ["Technology"]
Description = "security group vs network acl"
Tags = ["aws"]
date = "2019-02-26T12:40:37+08:00"
menu = "AWS"
title = "security group vs network acl"
toc = true
+++

## Difference between Security Groups and Network Access Control List (NACL) ##

### Scope ###
- Network ACLs are applicable at the subnet level, so any instance in the subnet with an associated NACL will follow rules of NACL

- Security groups has to be assigned explicitly to the instance.
This means any instances within the subnet group gets the rule applied.

### State: Stateful or Stateless ###

- Security groups are stateful.If you allow an incoming port 80,the outgoing port will be automatically opened
- Network ACLs are stateless .If you allow an incoming port 80, you would also need to apply the rule for outgoing traffic.


### Allow or Deny ###

- Security group supports allow rules only (by default all rules are denied)

- Network ACL supports allow and deny rules

### Rule Destination ###

- Security group rule allow CIDR, IP, Security group as destination.

- Network ACL rule only allow CIDR as destination.

### One or Multiple ###

- Subnet can have only one NACL, whereas Instance can have multiple Security groups.


### Effective Order ###

- Network ACL first layer of defense, whereas Security group is second layer of the defense for inbound/ingress traffic.

- Security group first layer of defense, whereas Network ACL is second layer of the defense for outbound/egress traffic.



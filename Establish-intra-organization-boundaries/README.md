## Establish intra-organization boundaries

* Controls that define boundaries and access controls between different organization units within your organization.


| Included Policy | Rationale | 
|-------------|-------------|
|[Deny resource access if the resource belongs to a specific organizational unit.](Deny-resource-access-if-the-resource-belongs-to-a-specific-organizational-unit.json) | Restrict actions for resources within the 'Suspended' Organizational Unit (OU) to only a privileged role by attaching this policy at the OU level. Note: If you would like to restrict AWS services such as security or logging services, you will need to remove the 'aws:PrincipalIsAWSService': 'false' condition.|
|[Deny access to resources in an organizational unit, except for principals from the same or specified organizational unit.](Deny-access-to-resources-in-an-organizational-unit,except-for-principals-from-the-same-or-specified-organizational-unit.json) | Restrict access to specified AWS services for all principals except those in a specific OU path, helping to grant broader access to new services in development.|

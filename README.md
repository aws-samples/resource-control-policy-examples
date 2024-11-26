## Resource Control Policy examples
------------------------------------------------------------------------------

**Resource control policies in this repository are shown as examples. You should not attach RCPs without thoroughly testing the impact that the policy has on resources in your accounts. Once you have a policy ready that you would like to implement, we recommend testing in a separate organization or OU that can be represent your production environment. Once tested, you should deploy changes to test OUs and then progressively deploy the changes to a broader set of OUs over time.**

[Resource control policies (RCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html)are meant to be used as coarse-grained preventative controls, and they don’t grant access. You must still attach [identity-based or resource-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html) to IAM principals or resources in your accounts to actually grant permissions. The effective permissions are the logical intersection between the SCP/RCP and an identity policy or the SCP/RCP and a resource policy. You can get more details about RCP effects on permissions [here](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html#rcp-effects-on-permissions). 

An [Resource control policy (RCP)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html), when attached to an AWS organization root, organization unit, or an account offers a central control over the maximum available permissions for resources in your organization, organization unit or an account. As an RCP can be applied at multiple levels in an AWS organization, understanding how [RCPs are evaluated](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps_evaluation.html) can help you write RCPs that yield the expected outcome. 
 
We recommend that you organize accounts using [OUs based on function](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/benefits-of-using-ous.html#group-similar-accounts-based-on-function), compliance requirements, or a common set of controls rather than mirroring your organization’s reporting structure. For more details, reference: [Design principles for your multi-account strategy.](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/design-principles-for-your-multi-account-strategy.html)


 


## This  repository
------------------------------------------------------------------------------
The example policies are divided into different categories based on the type of control. These examples do not represent a complete list and are intended for you to tailor and extend to suit the needs of your environment. This folder contains examples of RCPs that help controls on [services supported by RCPs.](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html#rcp-supported-services)

**Note** : The [RCPFullAWSAccess](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps_examples.html#example-rcp-full-aws-access)  policy is automatically attached to the organization root, every OU, and every account in your organization, when you enable resource control policies (RCPs). This default RCP allows all principals and actions access to pass through RCP evaluation. You can make use of Deny statements to restrict access to resources in your organization. You still also need to grant appropriate permissions to your principals by using identity-based or resource-based policies.

* **[Data perimeter guardrails](https://github.com/aws-samples/data-perimeter-policy-examples)** : Enforce preventive controls that help ensure only your trusted identities are accessing trusted resources from expected networks.

* **[Establish intra-organization boundaries](Establish-intra-organization-boundaries/Establish-intra-organization-boundaries.md)**: Controls that define boundaries and access controls between different organization units within your organization.

* **[Resource access method restrictions](Resource-access-method-restrictions/Resource-access-method-restrictions.md)**: Controls that enforce specific rules on the methods used to access your organization's resources, ensuring that only compliant access channels are permitted. 

* **[Limit access to trusted OIDC Identity provider](Limit-access-to-trusted-OIDC-Identity-provider/Limit-access-to-trusted-OIDC-Identity-provider.md)** : Controls that govern the authentication and authorization mechanisms used by external identity providers (IdPs) to grant access to your organization's resources.

* **[Service specific controls](Service-specific-controls/Service-specific-controls.md)**: Controls that define baseline security requirements or guidelines that are implemented across individual AWS services to ensure a standardized approach to security and compliance.
  
* **[Controls that can be implemented using either SCP or RCP](Controls-that-can-be-implemented-using-either-SCP-or-RCP/Controls-that-can-be-implemented-using-either-SCP-or-RCP.md)**: Controls that can be enforced using either service control policies (SCPs) or resource control policies (RCPs). 
    * Note that SCPs affect only IAM principals that are managed by accounts that are part of the organization. RCPs impact the effective permissions of principals trying to access resources in a member account with an applicable RCP, regardless of whether the principals belong to the same organizations or not. 








## Top RCPs to get started with
------------------------------------------------------------------------------

If you are just starting to implement RCPs in your environment, consider our top recommended RCPs. 

* [Only trusted identities can access my resources (Identity perimeter objective).](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/identity_perimeter_rcp.json)
* [Restrict access to only HTTPS connections to your resources.](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps_examples.html#example-rcp-enforce-ssl)
* [Limit access to trusted OIDC Identity provider (If applicable)](Limit-access-to-trusted-OIDC-Identity-provider/Limit-access-to-trusted-OIDC-Identity-provider.md)



## Documentation links
------------------------------------------------------------------------------

* [Resource control policies (RCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html)

* [Blog: Introducing resource control policies (RCPs), a new type of authorization policy in AWS Organizations](https://aws.amazon.com/blogs/aws/introducing-resource-control-policies-rcps-a-new-authorization-policy/)

* [Data perimeter on AWS](https://aws.amazon.com/identity/data-perimeters-on-aws/)


## Security
See [CONTRIBUTING](CONTRIBUTING.md) for more information.

## License
This library is licensed under the MIT-0 License. See the LICENSE file.

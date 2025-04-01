## Limit access to trusted OIDC Identity provider

* OIDC federation can be used to give temporary security credentials to users who have been authenticated through an OpenID Connect compatible identity provider (IdP) to an IAM OpenID Connect (OIDC) identity provider in your AWS environment. This section outlines control examples that govern the authorization mechanisms used by external OIDC identity providers (IdPs) to grant access to your organization's resources.


| Service Provider | Rational | 
|-------------|-------------|
|[Enforce trusted OIDC providers](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4bc433ff6c4721049fc2eb542c89246343b5fb8a/resource_control_policies/identity_perimeter_rcp.json#L55C19-L55C49) | Limit access to sts:AssumeRoleWithWebIdentity to only federated identities associated with a specific OIDC provider.|
|[Enforce trusted OIDC tenants](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4bc433ff6c4721049fc2eb542c89246343b5fb8a/resource_control_policies/identity_perimeter_rcp.json#L39C19-L39C44) | Limit access to sts:AssumeRoleWithWebIdentity to only federated identities originating from your tenant of a trusted multi-tenant OIDC provider. |
|[Enforce only trusted GitHub Organizations when using Github's default issuer](GitHub-Actions.json) | Help ensure that only repositories and jobs from your tenant within the github's SaaS environment can assume roles in your AWS accounts using `AssumeRoleWithWebIdentity`. This control is effective when you're using the default issuer URL shared by all github enviroments of `tokens.actions.githubusercontent.com` and uses part of the "sub" (subject)  claim value to identify which tenant a given token calling `AssumeRoleWithWebIdentity` belongs to. For example, you can use StringLike`"repo:<octo-org>/*"`replacing <octo-org> with the expected value of your "enterprise slug" to deny access to your AWS accounts from github actions, except when the github action belongs to your tenant of github.|
|[Deny the built in web identity providers (e.g., Facebook, Google, Login with Amazon, Cognito)](Deny-built-in-web-identity-providers.json) | Deny the four built-in web identity providers (Google, Facebook, AWS Cognito and Login with Amazon) from assuming roles in your AWS accounts using `AssumeRoleWithWebIdentity`. |



### Specific example controls for tenancy within multi-tenant OIDC providers with a shared issuer URL

[Enforce only specific tenants within providers with a shared issuer URL](Shared-Issuers.json) 

Some third party services that integrate with AWS using OIDC use a shared "issuer" (iss) URL for all their tenants within their service. The use of a shared issuer URL means that IAM roles configured to use these third party services' OIDC integrations require checks with condition keys in their role trust policies to ensure that other tenants from those third party services beyond what you intend cannot assume your IAM roles and access your AWS resources.

These example RCP statements when applied help ensure that only your tenant from these third party services that integrate with AWS using OIDC can assume your roles and access your resources, even when a role trust policy is misconfigured and is not checking for the tenancy. The [sample policy](Shared-Issuers.json) has one statement per a service's issuer URL with the example controls based on their documentation to help ensure that only your tenant from their platform can assume your IAM roles. The placeholder value to identify tenancy with these services located within the angle brackets, "<>" , is the term used by each services' documentation. To use the [sample policy](Shared-Issuers.json), you only need to use the statements for the third party services that you wish to use in your AWS organization.


Here is a list of the service providers covered in this sample, and their documentation:


| Provider                          | Documentation                                                                                          | Issuer/OIDC Provider URL                                              |    Policy Statement in [Shared-Issuers.json](Shared-Issuers.json)         |
|-----------------------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|
| Terraform Cloud                   | [Terraform Docs](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/aws-configuration) | https://app.terraform.io | EnforceTrustedOIDCTenantTerraformCloud|
| GitLab                             | [GitLab Docs](https://docs.gitlab.com/ee/ci/cloud_services/aws/) <br> [ID Token Auth](https://docs.gitlab.com/ee/ci/secrets/id_token_authentication.html) | https://gitlab.com | EnforceTrustedOIDCTenantGitLabCLoud|
| IBM Turbonomic SaaS                | [IBM Docs](https://www.ibm.com/docs/en/tarm/8.13.0?topic=suaiuir-setting-up-aws-iam-role-turbonomic-saas-deployments) <br> [IBM Docs](https://www.ibm.com/docs/en/tarm/8.14.6?topic=turbonomic-setting-up-aws-iam-role-saas-deployments) <br> [Support Page](https://www.ibm.com/support/pages/turbonomic-saas-iam-role-setup) | https://rh-oidc.s3.us-east-1.amazonaws.com/22ejnvnnturfmt6km08idd0nt4hekbn7 <br> https://rh-oidc.s3.us-east-1.amazonaws.com/23e3sd27sju1hoou6ohfs68vbno607tr <br> https://rh-oidc.s3.us-east-1.amazonaws.com/23ne21h005qjl3n33d8dui5dlrmv2tmg <br> https://rh-oidc.s3.us-east-1.amazonaws.com/24jrf12m5dj7ljlfb4ta2frhrcoadm26 <br> https://oidc.op1.openshiftapps.com/2f785sojlpb85i7402pk3qogugim5nfb <br> https://oidc.op1.openshiftapps.com/2c51blsaqa9gkjt0o9rt11mle8mmropu |EnforceTrustedOIDCTenantIbmTurboNomic1 <br>EnforceTrustedOIDCTenantIbmTurboNomic2 <br>EnforceTrustedOIDCTenantIbmTurboNomic3 <br>EnforceTrustedOIDCTenantIbmTurboNomic4 <br>EnforceTrustedOIDCTenantIbmTurboNomic5 <br>EnforceTrustedOIDCTenantIbmTurboNomic6 <br>|
| Shisho.dev                         | [Shisho Docs](https://shisho.dev/docs/g/getting-started/integrate-apps/aws/)                        | https://tokens.cloud.shisho.dev | EnforceTrustedOIDCTenantShishoDev|
| Scalr                              | [Scalr Docs](https://docs.scalr.io/docs/aws)                                                        | https://scalr.io | EnforceTrustedOIDCTenantScalr |
| GitHub Audit Log Streaming         | [GitHub Docs](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/streaming-the-audit-log-for-your-enterprise#setting-up-streaming-to-amazon-s3) | https://oidc-configuration.audit-log.githubusercontent.com | EnforceTrustedOIDCTenantGithubLogStreaming |
| Pulumi                             | [Pulumi Docs](https://www.pulumi.com/docs/pulumi-cloud/oidc/provider/aws/)                          | https://api.pulumi.com | EnforceTrustedOIDCTenantPulumi |
| Buildkite                          | [Buildkite Docs](https://buildkite.com/docs/agent/v3/cli-oidc)                                      | https://agent.buildkite.com |EnforceTrustedOIDCTenantBuildKite |
| Upbound                            | [Upbound Docs](https://docs.upbound.io/all-spaces/legacy-spaces/multicloud-deploy/)                | https://proidc.upbound.io | EnforceTrustedOIDCTenantUpbound|
| GitHub Actions - Self Hosted Runners | [GitHub Actions Docs](https://github.com/actions/runner/blob/main/docs/checks/actions.md)           | https://vstoken.actions.githubusercontent.com | EnforceTrustedOIDCTenantGithubActionsSelfHosted|
| Vercel                             | [Vercel Docs](https://vercel.com/docs/security/secure-backend-access/oidc/reference)                | https://oidc.vercel.com | EnforceTrustedOIDCTenantVercel|
| Sandboxes.cloud                    | [Sandboxes Docs](https://docs.sandboxes.cloud/docs/cloud-resources-setup)                          | https://sandboxes.cloud | EnforceTrustedOIDCTenantSandBoxes|
| Datachain.ai                       | [Datachain Docs](https://dvc.org/doc/studio/user-guide/openid-connect)                             | https://studio.datachain.ai/api | EnforceTrustedOIDCTenantDataChain |
| Codefresh                          | [Codefresh Docs](https://codefresh.io/docs/docs/integrations/oidc-pipelines/)                      | https://oidc.codefresh.io | EnforceTrustedOIDCTenantCodeFresh |

This list may not include all possible third party service providers that use a shared issuer URL with their OIDC integration to access AWS accounts. Please review all relevant documentation from your service providers to ensure your security objectives are met when configuring an OIDC integration to AWS from your third party service providers.

Please note this sample with all providers, and all the included whitespace is larger than the RCP size limit. You can reduce the size of the policy by removing statements for unneeded providers and removing whitespace.

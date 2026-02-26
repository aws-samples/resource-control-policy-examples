## Service specific controls

* Controls that define baseline security requirements or guidelines that are implemented across individual AWS services to ensure a standardized approach to security and compliance.

**Note**: Some controls in this section can be implemented using either service control policies (SCPs) or resource control policies (RCPs) as mentioned in the rationale column next to the policy. Note that SCPs affect only IAM principals that are managed by accounts that are part of the organization. RCPs impact the effective permissions of principals trying to access resources in a member account with an applicable RCP, regardless of whether the principals belong to the same organizations or not.

**Amazon Simple Storage Service (S3)**

| Included Policy | Rationale | 
|-------------|-------------|
|[Enforce TLS version](S3-Enforce-TLS-version.json) | Require a minimum TLS version of 1.2 for access to S3 buckets.|
|[Deny users from deleting Amazon S3 Buckets or objects](S3-Deny-users-from-deleting-Amazon-S3-Buckets-or-objects.json) | Restrict users or roles in any affected account from deleting S3 bucket or objects. This control can be implemented using either SCP or RCP.|
|[Deny ACL disablement for all new buckets (bucket owner enforced)](S3-Deny-ACL-disablement-for-all-new-buckets-(bucket-owner-enforced).json)| Require that all new buckets are created with ACLs disabled. Note: When you apply this setting, ACLs are disabled and you automatically own and have full control over all objects in your bucket. This control can be implemented using either SCP or RCP.|
|[Deny users from modifying S3 Block Public Access (Account-Level)](S3-Deny-users-from-modifying-S3-Block-Public-Access-(Account-Level).json) |Deny users or roles in any affected account from modifying the S3 Block Public Access Account level settings.Note: When you apply block public access settings to an account, the settings apply to all AWS Regions globally. This control can be implemented using either SCP or RCP.|
|[Prevent S3 buckets from being made public (Bucket level)](S3-Prevent-S3-buckets-from-being-made-public-(Bucket-level).json) |Deny users or roles in any affected account from modifying the S3 Block Public Access bucket level settings. This control can be implemented using either SCP or RCP.|
|[Prevents usage of customer-provided encryption keys (SSE-C) for S3 buckets (Bucket level)](S3-Deny-SSE-C.json) |Deny the use of customer-provided encryption keys (SSE-C) across the organization. This security measure helps ensure all S3 bucket encryption remains under organizational control by denying the use of S3 with SSE-C.|
|[Prevents long term presigned URLs](S3-Prevent-long-term-presigned-url.json) |Deny the use of presigned URL with a signature age greater than the configured expiration time.|


**AWS Key Management Service (KMS)**

| Included Policy | Rationale | 
|-------------|-------------|
|[Require an AWS Key Management Service key policy limiting creation of AWS KMS grants to AWS services](KMS-Require-an-AWS-Key-Management-Service-key-policy-limiting-creation-of-AWS-KMS-grants-to-AWS-services.json) |Prevent grants from being assigned directly to principals other than AWS service principals to reduce the opportunities for grant misuse.|
|[Deny AWS Key Management Service asymmetric key with RSA key material with key length of 2048 bits](KMS-Deny-AWS-Key-Management-Service-asymmetric-key-with-RSA-key-material-used-for-encryption-with-key-length-of-2048-bits.json) |Stronger RSA keys (3072-bit or 4096-bit) are recommended to provide better security.|
|[Require that an AWS KMS key is configured with the bypass policy lockout safety check enabled](KMS-Require-that-an-AWS-KMS-key-is-configured-with-the-bypass-policy-lockout-safety-check-enabled.json) |Deny bypassing the KMS key policy lockout safety check when creating a KMS key or updating its key policy, because bypassing this check increases the risk that a KMS key becomes unmanageable.|
|[Deny the accidental or intentional deletion of a KMS key and only allow specific roles to delete KMS keys.](KMS-Deny-the-accidental-or-intentional-deletion-of-a-KMS-key-and-only-allow-specific-roles-to-delete-KMS-keys.json)|Deny the accidental or intentional deletion of a KMS key and only allow specific roles to delete KMS keys.|

**AWS Identity and Access Management (IAM)**

| Included Policy | Rationale | 
|-------------|-------------|
|[Restrict outbound federation to approved audiences, signing algorithms, and token duration](IAM-Restrict-outbound-federation.json) |Restrict outbound federation to pre-approved audiences, enforce that IAM principals request tokens using signing algorithms compatible with your security requirements or the external services you integrate with (replace `<YOUR_SIGNING_ALGORITHM>` with valid values: RS256 or ES384), and restrict token lifetime. Note: While this policy governs audience values in sts:GetWebIdentityToken requests, it cannot enforce which external services are actually used—external services independently validate audience claims. The audience parameter is mandatory, so no null check is required for sts:IdentityTokenAudience.|


**Amazon Cognito**

| Included Policy | Rationale | 
|-------------|-------------|
|[Deny identity pool unauthenticated access](Cognito-Deny-identity-pool-unauthenticated-access.json) |Deny unauthenticated (guest) access to Cognito identity pools by blocking GetId, GetCredentialsForIdentity, and GetOpenIdToken operations for unauthenticated identities, ensuring only authenticated users can obtain credentials.|
|[Deny deletion and only allow specific roles](Cognito-Deny-deletion-and-only-allow-specific-roles.json) |Prevent accidental or intentional deletion of Cognito user pools or domains, except by specific privileged roles. Replace `[PRIVILEGED_ROLE]` with the actual role name authorized to perform deletions.|


**AWS STS**

| Included Policy | Rationale | 
|-------------|-------------|
|[Protect EKS Pod Identity Session Tags](STS-ProtectEKSPodIdentitiesTags.json) | Protect the session tags set by EKS pod identities. This RCP helps ensure that only AWS service principals can assume IAM role sessions with the EKS pod identity specific session tags, while allowing the role-sessions assumed by EKS pod identities to continue to set them as transitive session tags. This pairs well with a service control policy that restricts the ability for someone to use the iam:TagRole and iam:TagUser permission from creating tags on IAM roles and users with the expected keys and valued by EKS pod identities. The logic is that "Only an AWS service Principal can makea request for a role-session with any of those tags, or a session/role/user that already has one of those tags set".|









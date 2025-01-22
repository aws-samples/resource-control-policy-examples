## Service specific controls

* Controls that define baseline security requirements or guidelines that are implemented across individual AWS services to ensure a standardized approach to security and compliance.

**Note**: Some controls in this section can be implemented using either service control policies (SCPs) or resource control policies (RCPs) as mentioned in the rational column next to the policy. Note that SCPs affect only IAM principals that are managed by accounts that are part of the organization. RCPs impact the effective permissions of principals trying to access resources in a member account with an applicable RCP, regardless of whether the principals belong to the same organizations or not.


|Service | Included Policy | Rational | 
|------|-------------|-------------|
|Amazon Simple Storage Service (S3)|[Enforce TLS version](S3-Enforce-TLS-version.json) | Require a minimum TLS version of 1.2 for access to S3 buckets.|
|Amazon Simple Storage Service (S3)|[Deny users from deleting Amazon S3 Buckets or objects](S3-Deny-users-from-deleting-Amazon-S3-Buckets-or-objects.json) | Restrict users or roles in any affected account from deleting S3 bucket or objects. This control can be implemented using either SCP or RCP.|
|Amazon Simple Storage Service (S3)|[Deny ACL disablement for all new buckets (bucket owner enforced)](S3-Deny-ACL-disablement-for-all-new-buckets-(bucket-owner-enforced).json)| Require that all new buckets are created with ACLs disabled. Note: When you apply this setting, ACLs are disabled and you automatically own and have full control over all objects in your bucket. This control can be implemented using either SCP or RCP.|
|Amazon Simple Storage Service (S3)|[Deny users from modifying S3 Block Public Access (Account-Level)](S3-Deny-users-from-modifying-S3-Block-Public-Access-(Account-Level).json) |Deny users or roles in any affected account from modifying the S3 Block Public Access Account level settings.Note: When you apply block public access settings to an account, the settings apply to all AWS Regions globally. This control can be implemented using either SCP or RCP.|
|Amazon Simple Storage Service (S3)|[Prevent S3 buckets from being made public (Bucket level)](S3-Prevent-S3-buckets-from-being-made-public-(Bucket-level).json) |Deny users or roles in any affected account from modifying the S3 Block Public Access bucket level settings. This control can be implemented using either SCP or RCP.|
|Amazon Simple Storage Service (S3)|[Prevents usage of customer-provided encryption keys (SSE-C) for S3 buckets (Bucket level)](S3-Deny-SSE-C-(Bucket-level).json) |Deny the use of customer-provided encryption keys (SSE-C) across the organization. This security measure ensures all S3 bucket encryption remains under organizational control by blocking client-side encryption key management, reducing the risk of key loss and maintaining consistent encryption standards.|
|AWS Key Management Service (KMS)|[Require an AWS Key Management Service key policy limiting creation of AWS KMS grants to AWS services](KMS-Require-an-AWS-Key-Management-Service-key-policy-limiting-creation-of-AWS-KMS-grants-to-AWS-services.json) |Prevent grants from being assigned directly to principals other than AWS service principals to reduce the opportunities for grant misuse.|
|AWS Key Management Service (KMS)|[Deny AWS Key Management Service asymmetric key with RSA key material with key length of 2048 bits](KMS-Deny-AWS-Key-Management-Service-asymmetric-key-with-RSA-key-material-used-for-encryption-with-key-length-of-2048-bits.json) |Stronger RSA keys (3072-bit or 4096-bit) are recommended to provide better security.|
|AWS Key Management Service (KMS)|[Require that an AWS KMS key is configured with the bypass policy lockout safety check enabled](KMS-Require-that-an-AWS-KMS-key-is-configured-with-the-bypass-policy-lockout-safety-check-enabled.json) |Deny bypassing the KMS key policy lockout safety check when creating a KMS key or updating its key policy, because bypassing this check increases the risk that a KMS key becomes unmanageable.|
|AWS Key Management Service (KMS)|[Deny the accidental or intentional deletion of a KMS key and only allow specific roles to delete KMS keys.](KMS-Deny-the-accidental-or-intentional-deletion-of-a-KMS-key-and-only-allow-specific-roles-to-delete-KMS-keys.json)|Deny the accidental or intentional deletion of a KMS key and only allow specific roles to delete KMS keys.|













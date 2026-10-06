## AWS IAM Roles Anywhere

These resource control policy (RCP) examples provide guidance for protecting AWS Identity and Access Management Roles Anywhere configuration and restricting session creation to expected networks.

**Note:** RCPs do not grant permissions. They define the maximum permissions available to resources in member accounts. A principal must still receive permission from an applicable identity-based or resource-based policy, and an explicit deny in any applicable policy overrides an allow.

| Included policy | Rationale |
|---|---|
| [Deny certificate revocation list state changes except for a specific role](IAM-RolesAnywhere-Deny-crl-state-change-if-not-specifc-role.json) | Deny `rolesanywhere:DisableCrl` and `rolesanywhere:EnableCrl` unless `aws:PrincipalArn` matches the configured privileged role. This helps prevent unauthorized changes that could disrupt or weaken certificate revocation enforcement. Replace `${Account}` and `[PRIVILEGED_ROLE]` before deployment. |
| [Deny profile state changes except for a specific role](IAM-RolesAnywhere-Deny-profile-state-change-if-not-specifc-role.json) | Deny `rolesanywhere:DisableProfile`, `rolesanywhere:EnableProfile`, and `rolesanywhere:UpdateProfile` unless `aws:PrincipalArn` matches the configured privileged role. This helps protect profile availability and configuration. Replace `${Account}` and `[PRIVILEGED_ROLE]` before deployment. |
| [Deny trust anchor state changes except for a specific role](IAM-RolesAnywhere-Deny-trust-anchor-state-change-if-not-specifc-role.json) | Deny `rolesanywhere:DisableTrustAnchor`, `rolesanywhere:EnableTrustAnchor`, and `rolesanywhere:UpdateTrustAnchor` unless `aws:PrincipalArn` matches the configured privileged role. This helps protect certificate authority trust configuration and availability. Replace `${Account}` and `[PRIVILEGED_ROLE]` before deployment. |
| [Deny session creation from unexpected CIDR networks](IAM-RolesAnywhere-Deny-startsession-from-unexpected-networks-cidr.json) | Deny `rolesanywhere:CreateSession` when `aws:SourceIp` is outside the configured trusted CIDRs or the key is absent. Replace the documentation-only `203.0.113.0/24` range with the required trusted IPv4 and IPv6 ranges. |
| [Deny session creation outside approved VPC endpoints](IAM-RolesAnywhere-Deny-startsession-from-unexpected-networks-vpce.json) | Deny `rolesanywhere:CreateSession` when `aws:SourceVpce` differs from the configured endpoint IDs or the key is absent. This prevents session creation through public or unapproved endpoints. Replace the sample VPC endpoint ID and include every endpoint required by the deployment. |
| [Protect a privileged trust anchor with an approved VPC endpoint](IAM-RolesAnywhere-Deny-start-session-for-privileged-trust-anchor-if-not-from-expected-networks-vpce.json) | Deny `rolesanywhere:CreateSession` for one designated privileged trust anchor when the request does not use the approved VPC endpoint. Replace `${Region}`, `${Account}`, `${Privileged-Trust-Anchor-Id}`, and the sample VPC endpoint ID. Other trust anchors are unaffected by this statement. |

### Customization and deployment considerations

- Replace every template or sample value before deployment, including `${Account}`, `${Region}`, `${Privileged-Trust-Anchor-Id}`, `[PRIVILEGED_ROLE]`, `203.0.113.0/24`, and `vpce-0123456789abcdef0`.
- Update the `arn:aws` partition where required, and include the complete IAM path when configuring a privileged role ARN.
- The privileged-role conditions create exceptions to explicit denies; they do not grant the named role permission.
- `NotIpAddressIfExists` and `StringNotEqualsIfExists` in these deny statements also match when their condition key is absent. Consequently, the CIDR control denies requests without `aws:SourceIp`, and the VPC endpoint controls deny requests without `aws:SourceVpce`.
- Treat the CIDR and VPC endpoint controls as alternative network-perimeter patterns unless workloads are expected to satisfy both. Applying both broad controls independently requires a request to avoid both explicit denies.
- The trust-anchor-specific VPC endpoint control can make a privileged trust anchor stricter, but it cannot override or loosen another applicable deny.
- Test these policies in a dedicated account or organizational unit before progressively deploying them to production workloads.

### Related control

Consider pairing these RCPs with the [IAM Roles Anywhere session-tag protection SCP](https://github.com/aws-samples/service-control-policy-examples/blob/main/Service-specific-controls/AWS-IAMRolesAnywhere/Protect-IAMRA-Specific-Tags.json), which helps prevent IAM identities from using tags reserved for IAM Roles Anywhere sessions.

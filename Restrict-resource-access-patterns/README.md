## Resource access method restrictions

* Controls that enforce specific rules on the methods used to access your organization's resources, ensuring that only compliant access patterns are permitted. 


| Included Policy | Rationale | 
|-------------|-------------|
|[Restrict access to only HTTPS connections to your resources](Restrict-access-to-only-HTTPS-connections-to-your-resources.json) | Require that access to your resources only occurs on encrypted connections over HTTPS (TLS). This can help you prevent potential attackers from manipulating network traffic. This can help you prevent potential attackers from manipulating network traffic and trusted partners from accessing your resources over an unencrypted connection.|
|[Enforcing controls on AWS services that use Service principals to access your resources.](Enforce-controls-on-AWS-services-that-use-service-principals-to-access-your-resources.json) | Enforce confused deputy protection by denying AWS service principals access to resources unless they originate from your organization or explicitly trusted third-party accounts.|

# Security Without AWS External ID

Panther is a native AWS application and as such relies heavily on AWS IAM roles for managing access. Within a Panther deployment, every lambda function’s execution role is configured to have the minimum permissions necessary to perform its job. This may include permissions to assume roles in customer accounts.

Panther is also a Software-as-a-Service (SaaS) offering. SaaS services that assume IAM roles in customer accounts traditionally require the use of an external ID for security reasons. Panther does not, and this has raised eyebrows from some security-conscious customers.

Panther does not need to use external IDs because it is a single-tenant SaaS offering, meaning we deploy each customer’s Panther instance into a dedicated AWS account. Your Panther account’s AWS account ID serves as an alternative solution to the problem that external IDs exist to solve.



### What is an external ID and what problem does it solve?&#x20;

This AWS blog post offers an excellent explanation of what external IDs are and how they’re used to solve problems for multi-tenant offerings. To summarize, an external ID is a piece of information that can be added to a request to assume an IAM role. That role’s trust policy may (but is not required to) specify what external IDs it will accept when granting access to that IAM role. External IDs do not need to be secret or hard to guess to be effective, they merely need to be unique.

The primary function of external IDs is to solve for the “confused deputy” problem. The basic premise of the “confused deputy” problem in the context of AWS is: If a SaaS Service Provider has access to assume roles in multiple customer AWS accounts, it may be possible for one customer to trick the Service Provider into assuming a role in another customer’s account.



### Why Panther doesn't need to use an external ID for cross-account role access

The reason that Panther does not need an external ID is because it is not a multi-tenant SaaS offering. Every Panther SaaS deployment is deployed into a dedicated AWS account that hosts Panther for one individual customer.

The trust policies for the IAM roles that Panther uses specify which AWS account may assume that role. Since each customer’s Panther deployment is in a different AWS account, one customer’s Panther deployment does not have access to assume the roles another customer has deployed regardless of external ID.

**This is much more secure than using external IDs**. Even if a customer manages to completely compromise a Panther account to the point that they can directly access its data stores and execute its lambdas, they still could not scan another customer’s account. In a traditional multi-tenant model with external IDs, completely comprising the SaaS account would grant you full access to other customer’s accounts as you could simply change the external ID associated with your organization.



### Why not just add external IDs anyway?&#x20;

* **It doesn’t actually help.**&#x20;
  * It would be a veneer of security that does not actually protect our customers.&#x20;
* **Additional moving parts add complexity.**&#x20;
  * Adding an external ID without a valid reason has the risk of breaking a deployment. For example, there could be a valid security threat posed by scanning/log processing becoming unavailable for an extended period of time due to an external ID bug. While the risk of that is low, it’s not impossible; adding these external IDs would be an infinitely higher risk than not having them at all.&#x20;
* **It doesn’t make sense architecturally.**&#x20;
  * We allow self-hosted deployments. It does not make sense to allow an external ID in a Panther IAM role if one of our self-hosted customers could arbitrarily set the external ID their Panther deployment uses to match your external ID. We would not be able to guarantee external ID uniqueness across deployments since deployments cannot communicate with each other. There are possible workarounds, such as using UUIDs to avoid collisions, but properly supporting external IDs in any meaningful way would require architecture changes to support a feature that adds no security value to a Panther deployment.

Please reach out to Panther support if you have any questions on external IDs.

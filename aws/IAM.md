# Identity Access Management (IAM)
IAM allows you to manage users and their level of access to the AWS Console.

IAM gives you...
* Centralized control of your AWS account
* Shared Access to your AWS account
* Granular permission
* Identity federation (including active directory, facebook, linkedin)
* Provides temporary access for users/devices and services, as necessary
* Allows you to set up your own password rotation policy
* Integrates with other AWS services
* Supports PCI DSS Compliance (for the payments industry)

### Key Terms
* **Users** - End users (think people)
* **Groups** - A collection of users under one set of permissions
* **Roles** - You create roles and can then assign them to AWS resources
* **Policies** - A document that defines one (or more) permissions

### Key Points
* IAM is universal. It does not apply to regions at this time.
* The "root account" is simply the account created when first setup your AWS account. It has complete Admin access.
* New users have **no** permissions when first created. All new users must be assigned permissions.
* New users are assigned an **access key** and **secret access key** when first created. These can be used to access AWS via the APIs and the Command Line.
  * Note: You can only view the Secret Access Key once. Be sure to save it or, if lost, it will have to be regenerated.
* Via IAM, you can create and customise your own password rotation policies.


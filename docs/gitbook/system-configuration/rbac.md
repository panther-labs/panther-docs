# Role-Based Access Control

Role-Based Access Control (RBAC) gives Panther deployments fine-grained access control for their user accounts. A _role_ is a configurable set of permissions and every user is assigned to exactly one role.

## Default Roles

When you first deploy Panther, the following three roles are automatically created for you:

![](<../.gitbook/assets/image (1) (1) (1).png>)

* The "Admin" role will be automatically assigned to all existing users and has all available permissions.
* The "Analyst" role can use all the cloud security and log analysis features, but can't modify settings.
* The "AnalystReadOnly" role can view resources and alerts and Python code, but can't change anything.

## Customizing Roles

All roles (including the default ones above) are fully customizable by any user with `UserModify` permissions:

* You can create as many roles as you want (see the "Create New" button in the screenshot above)
* Roles can be renamed as long as the names are unique
* Role permissions can be changed as long as at least one user has `UserModify` permissions
* Roles can be deleted as long as no users are currently assigned to them

When you create or edit a role, you are shown the following screen:

![Role Edit](<../../../.gitbook/assets/rbac-role-edit (7) (7) (8) (1) (1) (3) (1) (1) (1) (7).png>)

{% hint style="info" %}
* Permission changes will not take effect until the affected user refreshes the browser where they are logged in to Panther or signs out and back in to Panther.
* If, after updating a user's permissions, the user continues to see an _**access denied** _ error,  double-check to see if they are missing another _Read_ permission that would be required for the page they are trying to access
{% endhint %}

# Jira

This page will walk you through configuring Jira as a Destination for your Panther alerts.

The Jira Destination requires the following:

* `Display Name`
* `Organization Domain`
* `Project Key`
* `Email`
* `Jira API Key`

When an alert is forwarded to a Jira Destination, a bug, story, or task is created and assigned to the specified assignee in the specified project.

## Example

![](<../../../.gitbook/assets/jira-issue (2) (2) (4) (3) (1) (3).png>)

### Integration fields

The `Organization Domain` is everything up to and including `.atlassian.net` in the project URL. In the case of our example, this would be `https://example.atlassian.net`.

The `Project Key` is the project identifier within your organization, which can be retrieved from the project settings page or by browsing your organizations projects and identifying the `key` column:

`https://example.atlassian.net/projects`

The `Email` is the Jira user's email of the account which has permissions to create the new issues with the corresponding `Jira API Key`. If possible, a service account should be created specifically for this purpose in order to ensure continuity.

The `Jira API Key` represents the API key of the user that will be used for creating the issues.

The `Assignee ID` is optional. If specified, issues created will automatically be assigned to this user. Otherwise, the issue will be `unassigned`.

The `Issue Type` is the label to tag for Jira issues.

![](<../../../.gitbook/assets/jira1 (9) (5) (1) (15).png>)

### Create a Jira API key

To create a new API key, please visit [https://id.atlassian.com/manage/api-tokens](https://id.atlassian.com/manage/api-tokens) while logged in as the user. click the `Create API Token` button, provide a memorable label, and click `Create`:

![](<../../../.gitbook/assets/jira-key1 (1) (1).png>)

After creating the token, you will have an opportunity to copy it. Jira warns this token is sensitive and you will not be able to access it again in the future. Click `Copy`:

![](<../../../.gitbook/assets/jira-key2 (1).png>)

Navigate back to the onboarding process inside Panther and paste in the key inside the appropriate field.

The assignee ID is the name of the user or group that the issue will be assigned to.

### Assignee ID

This field accepts the unique string ID called the user's `accountID`. You need to navigate to a user's public profile page to quickly extract the ID. For example, we can visit the organization's people page, find a user we want to assign, and click to get to their public profile:

`https://example.atlassian.net/jira/people/search`

Click on a desired user: ![](<../../../.gitbook/assets/jira-user1 (2) (2) (4) (5) (1) (3).png>)

Inspect the URL and copy the trailing string. This is the `accountId`. For example, in the following URL, `5f8f26dabd138600693d7fb8` would be the `accountId`:

`https://example.atlassian.net/jira/people/5f8f26dabd138600693d7fb8`

![](<../../../.gitbook/assets/jira-user2 (1).png>)

Navigate back to the onboarding process and paste this into the `Assignee ID` field.

Once you've filled out the form, click `Add Destination`. You may optionally send a test alert: ![](<../../../.gitbook/assets/jira2 (9) (6) (1) (14).png>) ![](<../../../.gitbook/assets/jira3 (1) (1).png>)

Click `Finish Setup` to finalize the destination configuration. This destination is now ready to receive alerts!

![](<../../../.gitbook/assets/jira4 (1) (1).png>)

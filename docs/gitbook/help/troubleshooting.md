# Troubleshooting

This page provides help for common issues you may encounter with Panther deployment or usage.

Don't see what you're looking for? [Contact support](./)

## Deployment

### First Invitation Email

Didn't receive the invitation email for the [first Panther user](../quick-start.md#first-login)?

1. Has the deployment finished successfully? The CloudFormation stack(s) should be `CREATE_COMPLETE`.
2. Check your spam folder - the email is sent from `no-reply@verificationemail.com`
3. From the CloudFormation web console, double-check the `FirstUserEmail` parameter in your stack - you may have mistyped the email. If so, read on.
4.  (Optional) Check the Panther CloudWatch dashboards for any errors and/or

    search the `/aws/lambda/panther-cfn-custom-resources` for error messages to help identify the root cause of the issue.

If you didn't receive the invite for whatever reason, you'll need to send another invite manually.

1. Download the appropriate `invite` opstool binary from the [latest release assets](https://github.com/panther-labs/panther/releases)
2. Run the tool, e.g.: `./invite-darwin-amd64` - it will prompt you for the user name and email

If your email already exists in Panther, you can try adding an email suffix: `user+suffix@example.com`

### No Rules or Policies

Every Panther deployment should automatically install many Python rules and policies. If you don't see any on your first login, there may have been an issue downloading them during deployment.

To fix:

1. Download the [analysis set](https://github.com/panther-labs/panther-analysis/releases/latest/download/panther-analysis-all.zip) manually
2. Login to your Panther Console and navigate to Cloud Security > Policies (or Log Analysis > Rules)
3. Click "Create New" and choose "Bulk". Upload the zipfile you downloaded in step 1.
4.  (Optional) Check the Panther CloudWatch dashboards for any errors and/or

    search the `/aws/lambda/panther-cfn-custom-resources` for error messages to help identify the root cause of the issue.

## Cloud Security

### My Resources Haven't Updated

When configuring Cloud Security for a given account, the default scan interval is 24h.

To reduce this interval, onboard CloudTrail data from the given account, or configure [real-time events](../data-onboarding/setup-cloud-accounts.md#configure-real-time-monitoring).

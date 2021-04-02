# FAQ

This page includes some commonly asked questions about using Panther and their answers.

## Writing Detections

### How does Panther handle errors on code / exceptions and do we get visibility when things fail?

If a rule raises an exception, then an alert gets sent to the same destination it would normally get sent to with details about the exception. The details of the exception do get pushed to the data lake, in a separate table from the rule matches.

### What is the recommended way to do exception handling?

Generally, detections should raise exceptions when either the logs don’t look like they need to \(for example you believe a field MUST have a value and it doesn’t\) or when an external thing \(like an API call\) fails. In these cases, you can either raise an exception yourself or more commonly just allow the exception to happen without catching it. This will not affect the ability of other rules to run.

### Is there any secret management functionality for detections?

By default, the rules and policy engines can retrieve secrets from the AWS secrets manager. The permissions are scoped to secrets that have the prefix `panther-analysis` in the secret name.

## Managing detections

### What is the best strategy for automating the deployment of detections?

The best way for technical teams to manage detections is with our `panther_analysis_tool` combined with a CI/CD pipeline. The CLI tool can run unit tests on and upload your python detections directly to Panther. What we recommend is users fork our public detections repo and use the `panther_analysis_tool` in a CI pipeline that only allows merges when unit tests pass and automatically uploads changes to your deployment when merged to master.

Then, when you’re ready to pull our changes in, you can pull in our upstream changes and they’ll be deployed just like your changes

### Do users often have a dev and prod environment for detections?

Generally, we do not see a full dev/prod deployment just for testing detections. Most teams rely on the unit testing built into the UI or the `panther_analysis_tool` to ensure their detections are working properly. One thing we have seen is when new detections are written their destination is set to a specific “dev” destination \(e.g. a slack channel that is muted or a dummy email that no one watches\). Then after a few days, you can go check to see if it would have alerted on anything, and if so to a reasonable degree.

### I still think that having a separate dev panther might be the cleaner approach. How do other Panther customers tackle development and testing?

The majority of our customers rely primarily on the unit testing capability combined with a CI pipeline that enforces passing unit tests with a minimum number of unit tests per detection \(with the `--minimum-tests` flag\).


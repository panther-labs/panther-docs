# Configuring a Custom Domain

## Configuring a Custom Domain

{% hint style="info" %}
This documentation only applies to legacy Self-Hosted deployments of Panther. SaaS customers have custom domain access, but it is automatically configured and does not require the special configuration outlined below.
{% endhint %}

Out of the box, Panther ships with a self-signed certificate generated at deployment time. While this setup is better than not having SSL/TLS enabled at all on the web server, it is still far from best practice; especially for a security tool. **Panther strongly recommends you replace this self-signed certificate with a certificate issued by a trusted Certificate Authority (CA) before using Panther in a production environment**.&#x20;

This documentation describes the process of registering a domain through Amazon Web Services (AWS) Route53, but you may use any domain registrar.

### Register a domain

Note: If you already have a domain registered, or if you have an internal CA that manages certificates for your organization, you can skip this step

1. Navigate to the [Route53](https://console.aws.amazon.com/route53/home) console and click the **Registered domains** tab.
2. Click **Register domain**, and enter the name of the domain you'd like to register.
3. Click the checkmark icon to verify that the domain is available. AWS will suggest alternatives if it is unavailable.
4. Click **Add to cart** and then **Continue**.
5. On the next form, fill in the contact information. Be sure to enter an email address that you have access to so that you can verify the domain in a future step. When you're done, click **Continue**.
6. Agree to the terms and conditions, and click **Complete order**. If you have not registered a domain through Route53 before, you will receive a confirmation email.

The domain will take between 10 minutes and one hour to complete registration. You can continue to the next steps before the domain registration is complete.

### Get a signed certificate into AWS

1. Navigate to the[ AWS Certificate Manager (ACM) console](https://console.aws.amazon.com/acm/home). Be sure you are in the same region that Panther is deployed in.
2. Click **Request a certificate**.&#x20;
   * If you are using a private CA, you will need to follow the Import a certificate workflow.
3. Make sure the **Request a public certificate** option is selected and click **Request a certificate**.
4. Enter the name of the domain registered above, and click **Next**.
5. Click either **DNS Validation** or **Email validation**.&#x20;
   * In this example, we will use **Email validation**.&#x20;
6. Click **Next**.
7. Optionally add tags. Adding the tag `Application:Panther` will help group this certificate with the rest of the Panther product. When you are done adding tags, click **Review**.
8. Verify everything looks correct, then click **Confirm and request**.
9. You will receive an email shortly requesting verification of the certificate. Click the link in the email to confirm the certificate.&#x20;
   * After verifying the certificate request, you will see the status of the certificate switch from Pending validation to Issued.
10. Be sure to note the Amazon Resource Name (ARN) of the newly-created certificate, as you will need it in the next steps.

### Configure Panther

The next step is to configure Panther to use your new certificate and domain. This can be completed with either an active Panther account or a new Panther deployment.

1. Navigate to the [CloudFormation](https://console.aws.amazon.com/cloudformation/home) console.
2. Find the Panther master stack (called `panther` by default), select this stack, and click **Update**.
3. Select the **Use current template** option and click **Next**.
4. Find the **Parameters** section and update the following two parameters:
   * **CertificateArn**: Enter the full ARN of the ACM certificate created in step two. This can be retrieved from the ACM console.&#x20;
   * **CustomDomain**: Enter the domain name you registered during the first section of this documentation.
5. Click **Next** until you reach the final "Review" step.&#x20;
6. On the "Review" step, verify that your configuration is correct. Check the box next to _I acknowledge that AWS CloudFormation might..._, then click **Update stack**.

After clicking **Update stack**, Panther will update with your new certificate. The update should take a few minutes.

### Create an alias

Finally, you will need to create an alias or CNAME on your domain pointing to the load balancer's auto generated URL. If you're not using a domain registered within Route53, you should still generally be able to follow along with the steps below through your registrar's web console.

1. Navigate to the Hosted zones tab of Route53, and click the Hosted zone for your new domain.
2. Click **Create Record Set**.
3. In the popup, fill in the fields as follows:
   * **Name**: Leave this field empty.&#x20;
   * **Type**: `A - IPv4 address`&#x20;
   * **Alias**: Select `Yes`&#x20;
   * **Alias Target**: Fill in the URL of the Elastic Load Balancer from your Panther deployment. To get this value:
     1. Go to the **EC2** service on the AWS console.
     2. Click **Load Balancers** in the left sidebar menu.
     3. Select **web** (if not already selected).
     4. On the bottom of your screen, click the **Description** tab.&#x20;
     5. Locate the **DNS Name**. Copy the value from this field into the Alias Target field while creating an alias.
        * The value should be something like `web-1xxxxxxxx.us-east-1.elb.amazonaws.com.`
   * **Routing Policy**: Select `Simple`&#x20;
   * **Evaluate Target Health**: Select `No`
4. Click **Create**.

![](<../../../../.gitbook/assets/hosted-zone-alias (9) (10) (3) (1) (1) (11) (1) (1) (1) (10) (12) (11).png>)

You can now navigate to your new domain and reach the Panther web application over a signed and secure HTTPS connection.

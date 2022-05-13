# Custom Webhook

This page will walk you through configuring a Custom Webhook as a Destination for your Panther alerts.

A Custom Webhook Destination requires only a `URL` to the service which can accept an HTTP `POST` request containing a `JSON` payload. This destination type is designed to allow Panther to communicate with other third-party integrations.

## Delivery and Ack

The webhook must accept and acknowledge Panther's `POST` request with an HTTP status code in the `2XX` range. If there were any network failures or non `2XX` codes, Panther will attempt to retry the request up to ten (10) times before permanent failure.

The webhook response body will be stored in the delivery status which can be viewed in the Alert Details page.

In the event of a permanent delivery failure, Panther logs and provides workflow continuity by allowing the alert to be manually re-sent by visiting the Alert Details page and viewing the Delivery Status section.

## Example

The following example demonstrates sending Panther alerts to a custom webhook which forwards the payload to a simple [Node.js](https://nodejs.org/en/) server proxied via [Ngrok](https://ngrok.com/).

### Step 1

Create an ngrok account, install, and and start the service on port `8081`:

![](<../../../.gitbook/assets/webhook1 (1).png>)

### Step 2

Install Node.js and paste the following snippet into `webhook.js`:

```javascript
const http = require('http')
const util = require('util')
const port = 8081

const requestHandler = (req, res) => {

  if (req.method === 'POST') {
    let body = '';
    req.on('data', chunk => {
      body += chunk.toString();
    });
    req.on('end', () => {
      console.log(util.inspect(JSON.parse(body), false, null, true));
      res.statusCode = 200; // Must ack the request
      res.end("success"); // (Optional) response body
    });
  }
}

const server = http.createServer(requestHandler)

server.listen(port, (err) => {
  if (err) {
    return console.log('something bad happened', err)
  }

  console.log(`server is listening on ${port}`)
})
```

Open another terminal and start the Node.js server:

```bash
> node webhook.js
server is listening on 8081
```

### Step 3

Create a Custom Webhook Destination in Panther, copy either the `http` or `https` forwarding URL from the `ngrok` output in Step 1 and paste it into the form. Optionally, select the desired severity levels and click `Add Destination`:

![](<../../../.gitbook/assets/webhook2 (1).png>)

### Step 4

At this stage, the destination has been added and we can test the integration using a test payload. Click on `Send Test Alert`:

![](<../../../.gitbook/assets/webhook3 (1).png>)

Inspect the terminal running the `node.js` server to see the result!

![](<../../../.gitbook/assets/webhook4 (1).png>)

Click `Finish Setup` to finalize the destination configuration. This destination is now ready to receive alerts!

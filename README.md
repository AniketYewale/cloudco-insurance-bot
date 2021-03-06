# Cloud Insurance Co. - Main web site and chat bot

| **master** | [![Build Status](https://travis-ci.org/IBM-Cloud/insurance-bot.svg?branch=master)](https://travis-ci.org/IBM-Cloud/insurance-bot) |
| ----- | ----- |
| **dev** | [![Build Status](https://travis-ci.org/IBM-Cloud/insurance-bot.svg?branch=dev)](https://travis-ci.org/IBM-Cloud/insurance-bot) |

This repository is part of the larger [Cloud Insurance Co.](https://github.com/IBM-Cloud/cloudco-insurance) project.

# Overview

[![Policy Bot](./design/video-cap.png)](https://vimeo.com/165460548 "Policy Bot Concept - Click to Watch!")

# Deploy
In order to deploy the full set of microservices involved, check out the [insurance-toolchain repo][toolchain_url]. Otherwise, you can deploy just the app by following the steps here.

## Running the app on IBM Cloud

1. If you do not already have a IBM Cloud account, [sign up here][bluemix_reg_url]

2. Download and install the [Cloud Foundry CLI][cloud_foundry_url] tool

3. The app depends on the [Catalog](https://github.com/IBM-Cloud/insurance-catalog) and [Orders](https://github.com/IBM-Cloud/insurance-orders) microservices. Make sure to deploy them first.

4. Clone the app to your local environment from your terminal using the following command:

  ```
  git clone https://github.com/IBM-Cloud/insurance-bot.git
  ```

5. `cd` into this newly created directory

6. Open the `manifest.yml` file and change the `host` value to something unique.

  The host you choose will determinate the subdomain of your application's URL:  `<host>.mybluemix.net`

7. Connect to IBM Cloud in the command line tool and follow the prompts to log in. Download and setup [IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started)

    ```
    ibmcloud login
    ```
    Use `ibmcloud target --cf` to set org and space; Run `ibmcloud regions` to find API endpoints.

8. Create a Cloudant service in IBM Cloud

    ```
    ibmcloud cf create-service cloudantNoSQLDB Lite insurance-bot-db
    ```

9. Create a Conversation service in IBM Cloud

    ```
    ibmcloud cf create-service conversation free insurance-bot-conversation
    ```

10. Push the app to IBM Cloud

    ```
    ibmcloud cf push --no-start
    ```

11. Define a variable pointing to the Catalog API deployment.

    ```
    ibmcloud cf set-env insurance-bot CATALOG_URL https://your-insurance-catalog.mybluemix.net
    ```

12. Define a variable pointing to the Orders API deployment.

    ```
    ibmcloud cf set-env insurance-bot ORDERS_URL https://your-insurance-orders.mybluemix.net
    ```

13. Start your app

    ```
    ibmcloud cf start insurance-bot
    ```

And voila! You now have your very own instance of the app running on IBM Cloud.

## Run the app locally

1. If you do not already have a IBM Cloud account, [sign up here][bluemix_reg_url]

2. If you have not already, [download Node.js][download_node_url] and install it on your local machine.

3. The app depends on the [Catalog](https://github.com/IBM-Cloud/insurance-catalog) and [Orders](https://github.com/IBM-Cloud/insurance-orders) microservices. Make sure to have them running first.

4. Create a Cloudant service in IBM Cloud

    ```
    ibmcloud cf create-service cloudantNoSQLDB Lite insurance-bot-db
    ```

5. Create a Conversation service in IBM Cloud

    ```
    ibmcloud cf create-service conversation free insurance-bot-conversation
    ```

6. In the checkout directory, copy the file ```vcap-local.template.json``` to ```vcap-local.json```. Edit ```vcap-local.json``` and update the credentials for the Cloudant and Conversation services. You can retrieve the service credentials from the IBM Cloud console.

    ```
    cp vcap-local.template.json vcap-local.json
    ```

7. In the checkout directory, copy the file ```.template.env``` to ```.env```. Edit ```.env``` and update the credentials for the Cloudant and Conversation services. Refer to [this step](#importWorkspace) to get a workspace id.

    ```
    cp .template.env .env
    ```

8. Install the dependencies

    ```
    npm install
    ```

9. Run the app locally

    ```
    npm start
    ```

## Contribute

If you find a bug, please report it via the [Issues section][issues_url] or even better, fork the project and submit a pull request with your fix! We are more than happy to accept external contributions to this project if they address something noted in an existing issue.  In order to be considered, pull requests must pass the initial [Travis CI][travis_url] build and/or add substantial value to the sample application.

## Troubleshooting

The primary source of debugging information for your IBM Cloud app is the logs. To see them, run the following command using the Cloud Foundry CLI:

  ```
  $ cf logs insurance-bot --recent
  ```

For more detailed information on troubleshooting your application, see the [Troubleshooting section](https://console.bluemix.net/docs/get-support/ts_overview.html#ts-overview) in the IBM Cloud documentation.

## License

See [License.txt](License.txt) for license information.

[toolchain_url]: https://github.com/IBM-Cloud/insurance-toolchain
[bluemix_reg_url]: http://ibm.biz/insurance-store-registration
[cloud_foundry_url]: https://github.com/cloudfoundry/cli
[download_node_url]: https://nodejs.org/download/
[issues_url]: https://github.com/IBM-Cloud/insurance-bot/issues
[travis_url]: https://travis-ci.org/

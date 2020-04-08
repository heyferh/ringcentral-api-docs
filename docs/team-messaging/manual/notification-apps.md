# Notification Apps

Team Messaging Notification Apps are a specialized form of app that perform the following functions for customers and developers.

1. Notification Apps help convert events emitted by a service into messages that are delivered to a team.
2. They provide developers with the means of promoting these apps directly to RingCentral Team Messaging and Glip users.
3. They allow developers to embed a setup form directly into the installation flow so that customers can make use of their integration more readily.

## Creating a Notification App

Creating a Notification App is identical in most respects to [creating an app](../../../basics/create-app/) of any other type. Notification Apps do however have a few additional fields that need to be populated by the developer in order to deliver their functionality as designed and intended. 

### Display Preferences

An app's "display preferences" determine how the app will be represented within RingCentral's Team Messaging Client.

<img src="../display-prefs.png" class="img-fluid" style="width:30%">

The preferences are:

* **Display Name** - the name that will appear in a list, or in a message stream
* **App Icon** - a square image that will be used in a list, or in a message stream

### Configuration Preferences

Configuration preferences contains information that drives the setup process for your app/integration. 

<img src="../config-prefs.png" class="img-fluid" style="width:30%">

Configuration preferences include:

* **Installation instructions** - this text will appear when a user elects to read the "manual installation instructions" associated with your app.
* **iFrame URL** - if your app supports the customer's ability to setup/configure your app via a form of somekind, enter the URL to that flow in this field. See "Automating App Setup" below.

#### {{ WEBHOOK_URL }} tokens

Setting up a notification apps requires a customer to register a special URL generated by the RingCentral platform with a third-party service. That service will then deliver messages to that URL for processing. Therefore, your manual installation instructions most contain the string `{{ WEBHOOK_URL }}`.

This special string will then be replaced in real-time with the URL the customer will need to register with the third-party. Furthermore, the team messaging client will also render a copy-to-clipboard button to make it easier for customers to install properly. 

Depending upon the service being integration with, you may not want the webhook URL rendered with a form element and a copy-to-clipboard button. To support other use cases, we support the following additional syntaxes.

Suppose there exists the following webhook URL -- the table below shows what each token will output:

    https://hooks.glip.com/webhook/v2/3053f6cf-b6de-418c-xxxx-2eb222cdab4e

| Syntax | Output |
|-|-|
| `{{ WEBHOOK_URL }}` | The URL adorned with HTML to render a copy-to-clipboard button. |
| `{{ WEBHOOK_URL raw }}` | The original URL in plain text. |
| `{{ WEBHOOK_URL id_only }}` | 3053f6cf-b6de-418c-xxxx-2eb222cdab4e |
| `{{ WEBHOOK_URL base}}` | https://hooks.glip.com/webhook/v2 |

## Automating App Setup

To provide users of RingCentral Team Messaging with the best possible experience to install your app, developers will need to implement a simple user interface that performs the following functions:

1. Authenticate the user installing the app via OAuth.
2. Present the user with setup preferences relating to the events and notifications they would like to receive as messages inside of RingCentral Team Messaging. 
3. Install the corresponding webhook URL in the target service via an API. 

!!! warn "Third-party service prerequisites"

    To create a simple form to automate the setup of a notification app, the target third-party service in question must support the following:
    
    * OAuth for user authentication and authorization.
    * An API for creating, updating and deleting webhooks.

### Design and Development Guidelines

To accomplish the above, developers need to create what amounts to a mini-web application. This web app will then be embedded into the install flow via an iFrame. We recommend developers adhere to the following guidelines when building this app.

* Present only a simple form. Try to minimize or suppress entirely branding and other UI-elements that might distract the user from completing the installation process.
* Do not display a submit button. The submission of your form will be triggered via a javascript callback. You should not display a button to submit your form.
* Limit your app to a single screen. Other than the step in which you present the user with a button to login, limit your setup form to a single page.
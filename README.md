# How to secure our applications using the Datawiza Access Broker 

This tutorial will show you how to use the Datawiza Access Broker ([DAB](https://datawiza.com/access-broker)) to implement a Zero Trust architecture for your applications. We will deploy the DAB to proxy to a simple Flask application and will use **Microsoft Azure Active Directory** as our Identitiy Provider (IdP) to implement Single Sign on (SSO). The DAB provides a unified authentication and authorization layer which is decoupled from the application itself. It can be deployed both on-premise and on the cloud as a container. After deploying the DAB, we will also see how we can implement access control policies on a URL-level. 

---

## Why Zero Trust?
A Zero Trust architecture helps to prevent successful data breaches by eliminating the concept of "trust" from an organization completely. It relies on the notion that everything within an organization's network cannot be trusted (say, connections from a VPN to a company resource). Whenever a user wants access to a specific resource, we make sure we authenticate and verify every step of the way. 

### Datawiza Access Broker ([DAB](https://datawiza.com/access-broker)) benefits

The DAB provides us with the ability to:
* Enable SSO with an Identity Provider (Azure AD, Okta) automatically
* Enable remote work without using a Virtual Private Network
* Enable a fine-grained URL-level access control based on a user's attributes

---

## Architecture
The Datawiza Access Broker is an **identity-aware reverse proxy** that sits in front of our applications. Traffic reaches the DAB first, and is then proxied to our app if allowed by the access policies we have specified. The DAB is managed by a centralized, cloud-based console: Datawiza Cloud Management Console (DCMC). The DCMC allows us to manage and configure the access control policies of multiple Access Brokers--regardless of whether they are running on-premise or in the cloud. 
![DAB deployment](./img/architecture_deployment.png)

### Deployment
The DAB can be deployed in one of two modes:
1. Sidecar mode: DAB deployed on the same server as the application
2. Standalone mode: DAB deployed on a different server than the application

---

## Preview
After learning a bit more about the architecture and benefits of the Datawiza Access Broker, let's see it in action for ourselves. In this tutorial, we will use the DAB to enable both SSO and granular access control for a simple Flask application serving static HTML. The Identity Provider we will use is Azure Active Directory. 
* Our Flask application will run on `localhost:3001`
* The DAB will run on `localhost:9772`. The traffic to our app will reach the DAB first, and then be proxied to our application. 
* The docker image for both the DAB and sample Flask application will be provided.
 
---

## Part 0: A quick look at our Flask app
Our Flask app is serving static HTML. To see what the page looks like, first source the virtual environment:
* `cd flask_app`
* `source blog-venv/bin/activate`

Then, run the application: `./app`.
> When visiting `http://localhost:3001`, you should see the following image:
![Flask App](./img/flask_app.png)

Eventually, once we set up the DAB to proxy to our app, we will be able to access our Flask application when visiting `http://localhost:9772`, where we should be prompted to sign with our Identity Provider (Azure Active Directory in this case).

--- 

## Part 1: Configure Microsoft Azure Active Directory
We have to register an OIDC Web application on the Microsoft Azure AD portal. The three values we get from our configuration (**Tenant ID**, **Application (client) ID**, **Client Secret**) will be used for later configuration in the Datawiza Cloud Management Console (DCMC). 

### Obtain Tenant ID
1. After registering for an account on Microsoft Azure, navigate to the Azure Active Directory tab in the menu. 
> Note down the `Tenant ID` on your Azure AD overview portal located in the `Tenant Information` box. 
![Azure Tenant ID](./img/azure_tenant_id.png)

### Register our app in Azure
2. Select `App Registrations` from the side bar and select `+ New registration`. Create an `Application` with the following fields:
* Name: e.g., Demo
* Supported account types: Accounts in this organizational directory only (Single tenant)
* Leave other fields as their default values
* Click `Register`

> Make sure to save the **Application (client) ID** after successfully registering your Application.
![Application Create](./img/create_application.png)

3. Making sure we are now in the application we have just created and are no longer in our `Default Directory`, select `Certificates & secrets` from the side bar. Create a new client secret by selecting `+ New client secret`. 
* Specify a name for the `client secret`
* Make the default 1 year

> Make sure to save the **Client Secret** after successfully creating a new client secret.
![Client Secret](./img/client_secret.png)

4. While staying in the `Demo` application we created, select `API permissions` from the side bar. `User.Read` should already be configured by default. Find and add `Group.Read.All` permissions under: `Add a permission` -> `Microsoft Graph` -> `Delegated Permissions` -> `Group` -> `Group.Read.All`. 
* After adding `User.Read` and `Group.Read.All`, make sure both permissions are "granted" for your directory. You can specify this option by selecting the `Grant admin consent for Default Directory` button. 

![API Permissions](./img/api_permissions.png)

5. Select `Authentication` from the side bar. `+ Add a platform`, and select `Web` under `Web Application`. 

6. Configure `Web` with the following values:
* Redirect URLs: `http://localhost:9772/login/oauth2/code/azure`
* You can leave `Logout URL` with its default value
* Make sure both `Access tokens` and `ID tokens` are allowed underneath `Implicit grant`

![Configure Web](./img/configure_web.png)



















Kirk part of Federation group
At end, have a member of klingon group try to access transporter

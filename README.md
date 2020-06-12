
# How to secure our applications using the Datawiza Access Broker 

---
This tutorial will show you how to use the Datawiza Access Broker ([DAB](https://datawiza.com/access-broker)) to implement a Zero Trust architecture for your applications. We will deploy the DAB to proxy to a simple Flask application and will use **Microsoft Azure Active Directory** as our Identitiy Provider (IdP) to implement Single Sign on (SSO). The DAB provides a unified authentication and authorization layer which is decoupled from the application itself. It can be deployed both on-premise and on the cloud as a container. After deploying the DAB, we will also see how we can implement access control policies on a URL-level. 

---

## Why Zero Trust?
A Zero Trust architecture helps to prevent successful data breaches by eliminating the concept of "trust" from an organization completely. It relies on the notion that everything within an organization's network cannot be trusted (say, connections from a VPN to a company resource). Whenever a user wants access to a specific resource, we make sure we authenticate and verify every step of the way. 

### DAB benefits

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

After learning a bit more about the architecture and benefits of the Datawiza Access Broker, let's see it in action for ourselves. 

- Show Flask app running
- Show Azure IdP config, etc...

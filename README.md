# IDP Initiated Authentication with OIDC SP login. 

## Purpose:
This repo contains steps and source code for the redirect hub demo.

### Authentication Flow:
 ![IDP Initiated flow](/IDP-Auth-authorize-SequenceDiagram.jpg) 

### Setup:

#### Auth0 Setup:

1. Create a federated enterprise connection to a SAML IDP in Auth0. This IDP should support IDP initiated Authentication and pass *SAMLResponse* with *RelayState* containing the downstream applications URL to Auth0. Let’s say the name for this connection is SAMLIDP

2. Create a redirect hub application in Auth0 – In the sample this application runs at http://localhost:3000. 
    1. Name: Redirect Hub
    2. Allowed Callback URLs: http://localhost:3000/callback
    3. Connections: Make sure SAMLIDP is a an active connection for this application
    4.	On the settings page for this application go to Show Advanced Settings, click it to show advanced sections. Click on the OAuth tab and turn off “OIDC Conformant”. Turning this flag off allow allows this application to prime the IDP initiated session such that SSO can be achieved. Make sure you click on Save under Advanced Settings

3.	Create the Service Provider Application as an SPA in Auth0. This application runs at http://localhost:3001
    1.	Name: Service Provider
    2.	Set “Allowed Callback URLs”, “Allowed Web Origins”, “Allowed Origins”, “Allowed Logout URLs” to http://localhost:3001 
    3.	Connections: Make sure SAMLIDP is an active connection for this application

4.	Update the IDP initiated authentication settings for the enterprise connection created at Step 1 – 
    1.	Select the Default Application: Redirect Hub
    2.  Response Protocol: OpenID Connect
    3.	Query String: response_type=code&scope=openid profile&redirect_uri=http://localhost:3000/login

### Application Source Code
1. [Redirect Hub](/redirecthub)
    - Make note of the Client ID, Client Secret, Domain of the `Redirect Hub` for setting the Environment variables for this application      
2. [SPA](/spa)
    - Make note of the Client ID, Domain of the `Service Provider` application for setting the Environment variables for this application

```
Note: This is just POC sample code and is not production ready. The POC was done to demo IPD Initiated login followed by OIDC SP login using SSO.
```
 

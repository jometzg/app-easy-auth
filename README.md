# Azure App Service Easy Autyh
A demonstration of how to amend an App Service protected with Easy Auth to be called remotely

## The Need
Sometimes it is useful to call APIs hosted in Azure App Service that has been configured to use "Easy Auth" https://learn.microsoft.com/en-us/azure/app-service/overview-authentication-authorization. 

By default Easy Auth allows the web application itself to be authenticated in a browser. This redirects to Microsoft Entra to then allow the user to enter their credentials. The application is then authenticated and any APIs hosted in the App Service can then be used. 

Sometimes it may be useful to call these APIs programmatically from code, VS Code or Postman. This opens up the possibility of driving the APIs hosted in App Service programmatically.

## The Problem
When attempting to access the App Service hosted APIs, firstly an authentication flow needs to be executed to gain an access token and then to present this access token to the API in the App Service. In the App Service's default configuration, this results in an error where the app reports that that the incorrect audience is being offered. This means that any API hosted in that App Service cannot be called.

## Understanding What is Happening
In an OAuth flow, the scope needs to be set for a request for an access token. The REST call for this is:

```
### get a bearer token
# @name gettoken
POST https://login.microsoftonline.com/{{directoryid}}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

client_id={{clientid}}
&scope=api://03ca27bb-e1db-493d-8a38-bff895e7d6c3/.default
&client_secret={{secret}}
&grant_type=client_credentials

### grab the access token
@authToken = {{gettoken.response.body.access_token}}
```

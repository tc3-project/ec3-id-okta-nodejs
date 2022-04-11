# El Centro Comercial de Cafe ![](./.common/logo.png?raw=true)
by Joel Mussman<br/>
original project concept by Joel Mussman and Justin Poole 

El Central Comercial de Cafe (a.k.a as CCDC or The Commercial Coffee Center) is the wholesale vendor
component of the The Caribbean Coffee Company (TC3) sandbox-universe.
TC3 is a cafe-operations project that was develope starting in 2018 as the basis for demonstrations
and classwork for almost every software development technology that you can work with, from databases through devices.
All participants can identify with the cafe scenario and have a feel for how it must work.
The core environment is kept simple: products, employees, and customers.

Applications are written to be used in a *customer-identity* or *workforce-identity* environment.
A customer-identity application is provided by an organization on their own infrastructure
to serve their direct customers (end-users).
TC3 is a customer-identity application.
A workforce-identity application is usually a cloud-based application sold to customers as individual
tenants for use by their employees, contractors, partners, etc.
CCDC is a workforce-identity application.

This copy of CCDC is an extremely simple example written in JavaScript & Express for NodeJS, and focuses
wholly on the integration of the Okta OIDC-middleware for providing identity in the Express pipeline.

CCDC is a hybrid workforce-identity product.
The CCDC customers need an application that can be linked to their identity provider, in this case Okta.
Unlike many tenant-based products where each customer is restricted to their own sandbox,
the back-end of CCDC is a single API shared by all customer tenants to manage orders in the CCDC system.
The point is that with their own IdP, customers manage their own assignments of employees to CCDC application.

Do not be confused by using Handlebars as the HTML template engine.
There is a single page *home* wrapped in the layout *main*.
Handlebars allows the application to avoid generating HTML boilerplate, in the handler for the
only page the application has *res.render('home', ...)* is used to pass an object with properties
that are merged into the *home* template.

<br>

# ccdc-id-okta-nodejs

## History

2022-04-08 - Original release.<br>

## Overview

The NodeJS Express environment is based around an app where routes and middleware are registered in
a pipeline which handles HTTP requests.
The Okta integration is a middleware package which is evaluated against every HTTP request.
It registers several endpoints including /login (GET) and /logout (POST).

The /login endpoint triggers an OpenID Connect (OIDC) /authorize call to Okta which begins the OIDC process.
This means that the user's browser is redirected to the Okta organization /authorize endpoint.
If the user is not authenticated, Okta will authenticate them.
Once authentication is complete, Okta will evaluate the user to see if they are assigned to
the application (authorized).
If authorized Okta will provide the identity to the application as an ID token.

OIDC-middleware uses an "authorization code flow" where a code is returned by having the browser
redirect back to the /authorization-code/callback endpoint in the application.
The middleware turns around and makes a direct HTTP call to the Okta organization /token endpoint
with the code, the application (client) id, and the client secret (password).
Okta provides the ID token on this call. 


## Project Setup

Run *npm install* to install the depdent package.

This example requires an Okta organization (a.k.a. tenant), of which a free one may be obtained at https://developer.okta.com.
In the organization register an OIDC application with the sub-type of a web application (a server that provides HTML pages).
Name the registration anything you like and set the sign-in redirect URI of the registration to http://localhost:8081/authorization-code/callback.
Everything else may remain the default values.

Copy the client id and secret.
Set the three environment variables (*set* for Windows, *export* for Mac):

```
set OKTA_ORG_URI=<the full URL to your Okta organization>
set OKTA_CLIENT_ID=<the client id for your application registration>
set OKTA_CLIENT_SECRET=<the secret password for your application registration>
```

Start the application and visit http://localhost:8081.
Click the login button and authenticate with a user assigned to the application in the Okta organization.
Notice the change in page display, now it shows the authenticated user's name and discount prices for wholesale products.

## Related Projects

Every project in https://github.com/tc3-project is related to the others in some manner.

## License

This project code is licensed under the [MIT license](./.common/LICENSE.md).

If you want to use these projects for demonstrations or as the basis of your own course labs, feel free.
I consider it to be a compliment and an honor that you want to use my sources, and I would much rather be
a collaborator than a competitor!
Feel free to contact me, and if you would, please give me credit when you use this.

## Project Details

This is part of of the larger Caribbean Coffee Company suite of components using different technologies and programming languages that fit into the TC3 project.
Learn more at [https://gitlab.com/tc3-project](https://gitlab.com/tc3-project).

## Support

If you found this project helpful, and and you would like to see more free projects like this,
then please consider
a contribution to *Joel's Coffee Fund* at **Smallrock Internet** to help keep the good stuff coming :)<br />

[![Donate](./.common/Donate-Paypal.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=XPUGVGZZ8RUAA)

<hr>
Copyright © 2019-2022 Joel A Mussman. All rights reserved.
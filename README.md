#### Create Serverless App

```shell
$ docker-compose run azure
root@c01c40041307:/home# npm i serverless -g
root@c01c40041307:/home# serverless create --template azure-nodejs --name temp
Serverless: Generating boilerplate...
  _______                             __
  |   _   .-----.----.--.--.-----.----|  .-----.-----.-----.
  |   |___|  -__|   _|  |  |  -__|   _|  |  -__|__ --|__ --|
  |____   |_____|__|  \___/|_____|__| |__|_____|_____|_____|
  |   |   |             The Serverless Application Framework
  |       |                           serverless.com, v1.11.0
  -------

Serverless: Successfully generated boilerplate for template: "azure-nodejs"
root@c01c40041307:/home# npm i
npm WARN package.json azure-nodejs@1.0.0 No repository field.
npm WARN package.json azure-nodejs@1.0.0 No license field.
npm WARN engine serverless-azure-functions@0.1.2: wanted: {"node":">= 6.5.0"} (current: {"node":"4.7.0","npm":"2.15.11"})
npm WARN deprecated minimatch@0.3.0: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated node-uuid@1.4.7: Use uuid module instead
npm WARN deprecated node-uuid@1.4.8: Use uuid module instead
serverless-azure-functions@0.1.2 node_modules/serverless-azure-functions
├── chalk@1.1.3 (supports-color@2.0.0, ansi-styles@2.2.1, escape-string-regexp@1.0.5, strip-ansi@3.0.1, has-ansi@2.0.0)
├── bluebird@3.5.0
├── fs-extra@2.1.2 (graceful-fs@4.1.11, jsonfile@2.4.0)
├── async@2.3.0
├── zip-folder@1.0.0 (archiver@0.11.0)
├── request@2.79.0 (tunnel-agent@0.4.3, forever-agent@0.6.1, aws-sign2@0.6.0, oauth-sign@0.8.2, caseless@0.11.0, is-typedarray@1.0.0, stringstream@0.0.5, aws4@1.6.0, isstream@0.1.2, json-stringify-safe@5.0.1, extend@3.0.0, uuid@3.0.1, qs@6.3.2, combined-stream@1.0.5, mime-types@2.1.15, tough-cookie@2.3.2, form-data@2.1.4, hawk@3.1.3, http-signature@1.1.1, har-validator@2.0.6)
├── ms-rest-azure@1.15.7 (async@0.2.7, uuid@3.0.1, adal-node@0.1.22, ms-rest@1.15.7, moment@2.18.1)
├── azure-arm-resource@1.6.1-preview (ms-rest@1.15.7)
└── lodash@4.17.4
```

- Setup Azure

```shell
root@c01c40041307:/home# azure login
info:    Executing command login
\info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin and enter the code YTSGTS to authenticate.
info:    Added subscription BizSpark
info:    login command OK
root@c01c40041307:/home# azure ad sp create -n hello-world -p 'strongpassword'
info:    Executing command ad sp create
+ Creating application hello-world
+ Creating service principal for application clientId
data:    Object Id:               {objectId}
data:    Display Name:            hello-world
data:    Service Principal Names:
data:                             clientId
data:                             http://hello-world
info:    ad sp create command OK
root@c01c40041307:/home# azure role assignment create --objectId {objectId} -o Owner -c /subscriptions/{subscriptionId}
info:    Executing command role assignment create
+ Finding role with specified name
/data:    RoleAssignmentId     : /subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleAssignments/6acb6170-785b-42fa-8a4d-3dc4d739d362
data:    RoleDefinitionName   : Owner
data:    RoleDefinitionId     : {roleDefinitionId}
data:    Scope                : {subscriptionId}
data:    Display Name         : hello-world
data:    SignInName           : undefined
data:    ObjectId             : {objectId}
data:    ObjectType           : ServicePrincipal
data:
+
info:    role assignment create command OK
root@c01c40041307:/home# azure ad sp show -c hello-world --json
[
  {
    "objectId": "objectId",
    "objectType": "ServicePrincipal",
    "displayName": "hello-world",
    "appId": "clientId",
    "servicePrincipalNames": [
      "http://hello-world",
      "clientId"
    ]
  }
]
root@c01c40041307:/home# azure account show
info:    Executing command account show
data:    Name                        : BizSpark
data:    ID                          : {subscriptionId}
data:    State                       : Enabled
data:    Tenant ID                   : {tenantId}
data:    Is Default                  : true
data:    Environment                 : AzureCloud
data:    Has Certificate             : No
data:    Has Access Token            : Yes
data:    User name                   : {email}
data:
info:    account show command OK

root@c01c40041307:/home# azureSubId=subscriptionId azureServicePrincipalTenantId=tenantId azureservicePrincipalClientId=clientId azureServicePrincipalPassword=strongpassword serverless deploy
Serverless: Packaging service...
Serverless: Logging in to Azure
Serverless: Creating resource group: hello-serverless-rg
Serverless: Creating function app: hello-serverless
Serverless: Waiting for Kudu endpoint...
Serverless: Parsing Azure Functions Bindings.json...
Serverless: Building binding for function: hello event: httpTrigger
Serverless: Packaging function: hello
Serverless: Syncing Triggers....Response statuscode: 200
Serverless: Running Kudu command del package.json...
Serverless: Uploading pacakge.json ...
Serverless: Running Kudu command npm install --production...
Serverless: Successfully created Function App
root@c01c40041307:/home# azureSubId=subscriptionId azureServicePrincipalTenantId=tenantId azureservicePrincipalClientId=clientId azureServicePrincipalPassword=strongpassword serverless invoke -f hello
Serverless: Logging in to Azure
"Hello World"
```
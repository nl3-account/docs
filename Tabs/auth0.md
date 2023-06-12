## Auth0 Integration
The Next Level3 Auth0 integration is designed to be used for any of your existing applications or sites which are using Auth0 for authentication. This integration will allow you to easily add Account Protection to any application that leverages Auth0 for authentication.
**Pre-requisites**
* Auth0 Application
* Next Level3 Company Account
* Signing Key created for an application in the Next Level3 Company Portal
### Adding Next Level3 Custom Auth0 Action
The first step to add an NL3 Account Protection Check to an existing application using Auth0 for authentication is to add a custom action which will then be added to your existing login flow.
### Adding an Auth0 Custom Action
a. After logging into the Auth0 management portal, navigate to ‘Actions -> Library’ from the main menu and click the “Build Custom” button to build a custom action.
<img src="/images/auth0/auth0 pic 1.png" />
b. Enter the following Information (replacing myApplication with the name of your application) in the Custom Action form and click Create.
* Name – myApplication Account Protection Check
* Trigger – Login/Post Login
* Runtime – Node 16 (Recommended)
<img src="/images/auth0/auth0 pic 2.png" />
c. In the code editor for the action, remove all the existing default code and replace it with the following NL3 Action code.
```
const https = require('https');
const nJwt = require('njwt');

function getLockStatus (jwt, api_host, api_path, event) {
    return new Promise((resolve, reject) => {
    var post_data = JSON.stringify({ "userIP": event.request.ip,
        "userDevice": event.request.user_agent,
        "userLocation": event.request.geoip.cityName,
        "integrationType": "auth0",
        "integrationData": event.request
        })
    var options = {
        host: api_host,
        port: "443",
        path: api_path,
        method: "POST",
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json',
            'x-nl3-authorization-token': jwt,
            'Content-Length': Buffer.byteLength(post_data)
        }
    }

    const req = https.request(options, (response) => {
        let chunks_of_data = [];

        response.on('data', (fragments) => {
            chunks_of_data.push(fragments);
        });

        response.on('end', () => {
            let response_body = Buffer.concat(chunks_of_data);
            resolve(response_body.toString());
        });
        response.on('error', (error) => {
            console.log("Error = " + error.message);
            reject(error);
        });
    });
        req.write(post_data);
        req.end();
    });
}

exports.onExecutePostLogin = async (event, api) => {
    if (event.client.client_id==event.secrets.CLIENT_ID) {
        var claims = {
        iss: event.secrets.APP_URI,
        aud: event.secrets.API_HOST,
        sub: event.user.name
        }
    let decodedDomainToken = Buffer.from(event.secrets.SIGNING_KEY, 'base64');
    var jwt = nJwt.create(claims, decodedDomainToken);
    jwt.setExpiration(new Date().getTime() + (60*5*1000)); //5 minute expiration to allow for SignUp
    jwt.setNotBefore(new Date().getTime() - (60*1*1000)); //Valid from 1 minute ago to account for minor time diffs
    var authToken = jwt.compact();

    if (authToken.length > 0) {
        const res = await getLockStatus (authToken, event.secrets.API_HOST, event.secrets.API_PATH, event);
        var result = JSON.parse(res);

            if(result) {
                console.log(JSON.stringify(result));
                if(result.locked) {
                    api.access.deny(event.secrets.LOCKED_MESSAGE);
                }
            } else {
                //Add logic for violations if required
            }
        }
    }
};
```
Once you have updated the code, click “Save Draft”. After doing this, your action code should look like the following:
<img src="/images/auth0/auth0 pic 3.png" />
d. Now you need to set up some secrets for the new custom action in order to configure it for your Auth0 protected application. This includes a secret for your NL3 signing key so that the action can check the lock status of users that are set up for Account Protection. You can retrieve your signing key from the Next Level3 Company Portal (https://company.nextlevel3.com) after you have added the application to the company portal and validated the associated domain for that application. 

To update secrets, click the ‘Key’ Icon to the left of the code editor called secrets and select the “Add Secret” button   
          
<img src="/images/auth0/auth0 pic 4.png" />

e. Enter to following secret key/value pairs one at a time in the ‘Define Secret’ form and click “Create”.

    - Key = LOCKED_MESSAGE
    - Value = Add a non-descript error message of your choosing such as ‘Login Failed’

    <img src="/images/auth0/auth0 pic 5.png" />

    - Key = SIGNING_KEY
    - Value = Paste your domain key provided by the Next Level3 company portal for the domain.
            
    - Key = CLIENT_ID
    - Value = The Auth0 Client ID for your application which can be found under the “Applications” menu in Auth0
            
    - Key = API_HOST
    - Value = Currently, the API host for production endpoints is api.nextlevel3.com

    - Key API_PATH
    - Value = Currently, the API path for production is /v1/AccountProtectionCheck

    - Key APP_URI
    - Value = This must match the value of the Domain URL you configured in the NL3 company portal (e.g. myapp.domain.com)
f. Next click the package icon labeled “Modules” to add some additional npm modules to the action. The only dependency that is not included by default is “njwt”. Click the ‘Add Dependency’ button, then enter “njwt” into the “Name” field and click the “Create” button. 

<img src="/images/auth0/auth0 pic 6.png" />

g. Next, click “Save Draft” from the code editor window to save the action.

<img src="/images/auth0/auth0 pic 7.png">

h. Click “Deploy” from the code editor window to deploy the action. After you click “Deploy”, a slider popup will appear letting you know that the action was successfully deployed.

i. Click “Add to Flow” from that slider pop-up.

<Info> If you missed the popup slider,  you can also navigate to “Actions -> Flows” and select the Login tile. </Info>
            
<img src="/images/auth0/auth0 pic 8.png" />

j. Click the “Custom” tab to list your custom actions. You should see the NL3 Account Protection action you just created in the list.

<img src="/images/auth0/auth0 pic 9.png" />

k. Drag and drop the NL3 Account Protection action into the flow.

    Your login flow should look similar to this now.
    
    <img src="/images/auth0/auth0 pic 10.png" />
    
    l. Click “Apply” to apply the flow changes.

    That’s it. You have now enabled Next Level3 Account Protection. After completing the second phase of integration (User Enablement) and enabling users,  you will be able to test out the lock/unlock capabilities in your site for users that have enabled their accounts for protection.

    <Tip> If you use IAM Roles and not IAM Users, please contact support to discuss the options for implementation and specific design considerations that may be required to implement this correctly using roles. </Tip>

The next step is to setup the Amazon EventBridge event rule that triggers the Lambda function for specific events. Here is a sample rule:

Reference:

[https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html)
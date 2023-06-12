## Microsoft Azure AD and O365 Integration
<img src="/images/microsoft b2c/azure1.png" />
This section will walk you through the required setup and configuration steps needed to enable the Next Level3 Cloud Identity solution in a Microsoft online environment. These steps include setting up and configuring a Microsoft AD FS Farm (if one does not already exist), setting up and configuring the Next Level3 Cloud Identity Account Protection Check by using a Microsoft AD FS Risk Assessment Model Plugin. If you have already federated your Microsoft Online services with AD FS, skip the pre-requisites and go directly to the plugin installation and setup.

Pre-requisites
  * Install and Configure an AD FS Server Farm
  * Choose Option A or B
    * Option A – use Option A if you want to install and configure AD FS yourself
    * Option B if you want to use an ARM template to set up an AD FS farm in Azure, and skip this step altogether if you already have an AD FS Server Farm 2016 or later.
### Option A - Install and Configure AD FS on Windows Server 2022 or 2019 on-premises
Installation and configuration of AD FS on Windows Service 2022 or 2019 is the first step. This can be done in an on premise or cloud hosted environment depending on your specific needs. To get started with this process follow the documentation found here:
**GUIDE FOR INSTALLATION, CONFIGURATION, AND DEPLOYMENT**
* [https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide](https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide)
* [https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/deploying-a-federation-server-farm](https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/deploying-a-federation-server-farm)
* [https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/deploying-federation-server-proxies](https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/deploying-federation-server-proxies)
### Option B - Install and Configure AD FS in Azure
**GUIDE FOR DEPLOYING IN AZURE INCLUDING ARM TEMPLATE**
_skip to “Step-by-step Instructions for Using ARM Template to Deploy AD FS Farm” for automated setup instructions_
      
* [https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs](https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs)

**Step-by-step Instructions for Using ARM Template to Deploy AD FS Farm**
  - Click Deploy to Azure in README.md for the following repository (https://github.com/Next-Level3/adfs-6vms-regular-template-based-server-2022)
  - Log into Azure as an account with permissions to deploy Virtual Networks, Load Balancers, and Virtual Machines
  - Fill in template parameters as follows:
    - Subscription – celect appropriate Subscription from drop-down.
    - Resource Group – create a new Resource group and select from drop-down.
    - Region – inherited from resource group (cannot edit)
    - Location – enter region/location for resources (e.g., East US)
    - Storage Account Type – choose appropriate option from drop-down.
    - Virtual Network Usage – select “new” from drop-down.
    - Virtual Network Name – enter name for new virtual network.
    - Virtual Network Resource Group Name – n/a.
    - Virtual Network Address Range – leave defaults unless changes are needed for your environment.
    - Internal Subnet Name – leave defaults unless changes are needed for your environment.
    - Internal Subnet Address Range – leave defaults unless changes are needed for your environment.
    - Dmz Subnet Address Range – leave defaults unless changes are needed for your environment.
    - Dmz Subnet Name – leave defaults unless changes are needed for your environment.
    - Addc01Nic IP Address – leave defaults unless changes are needed for your environment.
    - Addc02Nic IP Address – leave defaults unless changes are needed for your environment.
    - Adfs01Nic IP Address – leave defaults unless changes are needed for your environment.
    - Adfs02Nic IP Address – leave defaults unless changes are needed for your environment.
    - Wap01Nic IP Address – leave defaults unless changes are needed for your environment.
    - Wap02Nic IP Address – leave defaults unless changes are needed for your environment.
    - Adfs Load Balancer Private Ip Address – leave defaults unless changes are needed for your environment.
    - Add VM Name Prefix – leave defaults unless changes are needed for your environment.
    - Adfs VM Name Prefix – leave defaults unless changes are needed for your environment.
    - Wap VM Name Prefix – leave defaults unless changes are needed for your environment.
    - Add VMs Size – leave defaults unless changes are needed for your environment (recommend Standard_B2s for pre-production environments to save money).
    - Adfs VMs Size – leave defaults unless changes are needed for your environment (recommend Standard_B2s for pre-production environments to save money).
    - Wap VMs Size – leave defaults unless changes are needed for your environment (recommend Standard_B2s for pre-production environments to save money).
    - Admin Username – fill in with the desired username.
    - Admin Password – fill in with the desired password.
    - Click “Review + create”.
    - Click “Create”.
  - After resources are created, connect to wap01 via RDP over port 5000 by getting Frontend IP from the wap-lb resource.
    - Example Frontend IP Configuration.
    - Example Remote Desktop (RDP) Connection configuration.
  - Connect using the admin username and password configured in the corresponding deployment parameters.
  - Connect to other resources to configure them by using the RDP client on wap01 to any of the other servers for configuration.
  
  - Set up the domain and join ADFS servers (if the existing domain is not available for federation).
    - Connect to dc01 at 10.0.0.101 from wap01.
    - Install and configure Active Directory Domain Services by following these instructions (https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services–level-100-).
    - Connect to dc02 at 10.0.0.102 and install and configure it as a secondary domain controller.
    - Connect to adfs01 at 10.0.0.201 from wap01 and join it to the domain created in the previous steps (you may need to update the DNS servers in the network interface adapters to point to dc01 and dc02.
    - Connect to adfs02 at 10.0.0.202 from wap01 and join it to the domain created in the previous steps.
Pre-requisite 1 – Not required if O365 or other Microsoft Online services are already in use by your company.
  - Set up an Azure AD tenant and custom domain for your Active Directory domain that will be federated with Microsoft Online (O365, Azure AD, etc.).
Pre-requisite 2
  Azure AD Domain Setup
    - Create corresponding domain in Azure AD.
      - Connect and log in to https://portal.azure.com with admin credentials.
      - Search for “Azure Active Directory” and choose that service from the results.
      - Select “Manage Tenants” at the top of the “Overview” screen.
      - Click “Create” on the “Manage Tenants” screen.
      - Choose “Azure Active Directory” and click “Next: Configuration >”.
      - Fill in the details for the Domain created in the last section or an existing domain.
    - For “Initial domain name” use the domain prefix for your domain as you will be required to set up a “Custom domain” to get it to match the FQDN of your domain.
      - After entering the configuration details, select “Next: Review + create >”, then “Create”, and finally submit any captcha to complete the process.
      - After completion, click on the link to the new domain and sign in with the credentials used to create the domain.
      - Select “Custom domain names” from the side menu, then “Add custom domain” at the top of the resulting configuration screen.
      - Enter the FQDN of the existing domain you would like to federate through AD FS and click “Add custom domain”.
      - Create TXT or MX records in DNS for the domain to verify ownership.
      - After creating the records and confirming they are resolving, click “Verify”.
      - Do NOT click “Make Primary”
    - Create a user account in the new Azure AD Domain with the <domain>.onmicrosoft.com prefix and add the “Hybrid Identity Administrator” role to that user.
      - Search for “Azure Active Directory” and select that service.
      - In the top menu, select “New user”, “Create new user”.
      - Enter the user’s details on the configuration screens until reaching “Assignments”.
      - On the “Assignments” screen, select “+ Add role”.
      - Search for “Hybrid Identity Administrator” and check the box next to it before clicking “Select”.
      - Search for “Global Administrator” and check the box next to it before clicking “Select”.
      - Select “Next: Review + create >”, then “Create”.
      - Confirm the roles have been assigned under “Assigned roles”.
      - If not, click “Add assignments” and search for “Hybrid Identity Administrator” and “Global Administrator” again and click “Add”.
      - Click “Refresh” to confirm the role has been added.
        
        After the AD FS Farm servers are available and the Azure AD tenant and domain are set up, the next step is to configure AD FS on the server infrastructure and federate the Microsoft Online domain with the AD FS Farm, if these steps have not already been completed. If you have already set up “Azure AD Connect” to synchronize your on-premises domain with Azure AD but have not yet set up an AD FS server farm or federation, use Option B.

Pre-requisite 3. Configure AD FS for Single Sign-On
      
**Option A** - Assisted setup with new Azure AD Connect installation and configuration
  - Install and run “Azure AD Connect” on a domain joined server.
    - Turn off Internet Explorer’s (IE) Enhanced Security Configuration under “Server Manager” > “Local Server”.
    - Add https://secure.aadcdn.microsoftonline-p.com and https://login.microsoft.com to the “Trusted Sites” zone in IE.
    - Download “Azure AD Connect” from here or from the latest link provided after verifying the domain (https://www.microsoft.com/en-us/download/details.aspx?id=47594).
    - Install “Azure AD Connect” using the downloaded MSI.
    - Check “I agree to the license terms and privacy notice.” and then click “Continue.”
    - Select “Customize”.
    - Leave everything unchecked under “Install required components”, unless you desire additional customization, and then click “Install”.
    - On the “User sign-in” screen, select “Federation with AD FS”, then select “Next”.
    - On the “Connect to Azure AD” screen, enter the credentials for the “Hybrid Identity/Global Administrator” account created when completing the “Azure AD Domain Setup” steps (be sure to use the <domain>.onmicorosft.com username suffix) and click “Next”.
    - Re-enter the “Hybrid Identity/Global Administrator” account username with the <domain>.onmicrosoft.com suffix on the “Sign In” screen that pops up.
    - If required to update the password on the first login, do so.
    - On the “Connect your directories” screen, select the domain you wish to federate and click “Add Directory”.
    - On the “AD Forest Account” select “Create new AD account” and enter an “Enterprise Admin” username and password for the on-premises domain and click “OK”.
    - Select “Next” and under “Azure AD sign-in configuration” confirm the Active Directory UPN Suffix is displayed and that it shows “Verified” under “Azure AD Domain”.
    - Under “Select the on-premises attribute to use as the Azure AD username” select “userPrincipalName” which should be the default and select “Next”.
    - If necessary, filter the domains or OUs you wish to synchronize. If not, leave it as “Sync all domains and OUs”.
    - Under “Identifying Users”, leave the defaults unless you have users who exist in multiple directories (if this is the case, consult the Azure AD Connect installation instructions here https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-custom#select-how-users-should-be-identified-in-your-on-premises-directories), click “Next”.
    - If desired, filter synchronization for specific users or groups. If not leave the defaults and click “Next”.
    - Configure “Optional features” as needed or leave defaults and select “Next”.
    - Enter a “Domain Administrator” account for the domain in which AD FS will be deployed (the domain you are federating) and click “Next”.
  - Continue creating a new AD FS Farm
    - Select “Configure a new AD FS farm” and browse to a certificate file for the domain you will be using for your AD FS farm FQDN and enter the password for the PFX file you select (see certificate requirements here learn.microsoft.com/en-us/windows-server/identity/ad-fs/design/certificate-requirements-for-federation-servers).
    - Select the appropriate “SUBJECT NAME” from the certificate for your AD FS form and then enter the prefix you want to assign to your farm to ensure it matches the “SUBJECT NAME” defined in the certificate (unless using a wildcard certificate in which case the prefix you wish to use for the farm; e.g., adfs, sso, sts) and click “Next”.
    - Enter the server name or IP address under SERVER for your primary ADFS server, click “Add”, and after validation click “Next”.
    - Make sure the “Web Application Proxy” server resolves the AD FS farm FQDN to the internal IP address of the AD FS farm server by using internal DNS or the hosts file (typically found at C:\Windows\System32\drivers\etc\hosts). Also, include an entry for the FQDN of the host as produced by Active Directory (e.g., <hostname>.<subdomain>.<domain>.com)
    - Make sure the Azure AD Connect server, resolves a Web Application Proxy Hostname to the IP address of the Web Application Proxy in the DMZ using internal DNS or the hosts file (typically found at C:\Windows\System32\drivers\etc\hosts) and that WinRM is allowed through firewalls.
    - Enable PS-Remoting on the Azure AD Connect Server and the Proxy by running the following commands from an elevated PowerShell session.
      - On the proxy, run “Enable-PSRemoting -force” then “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <AADConnectServerFQDN>” replacing <AADConnectServerFQDN> with the FQDN for the Azure AD Connect server and confirming with “Y” when prompted, then, “Restart-service -name winrm”.
      - On the AD connect server, run “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <DMZServerHostname> -Force -Concatenate”, run “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <DMZServerIPAddress> -Force -Concatenate”, then “Restart-service -name winrm”.
      - Either change the internal facing network adapter for the proxy to “Private” or add firewall rules to allow WinRM from the Azure AD Connect server.
    - Enter the Hostname or IP address (if the Proxy server is not joined to a domain with a proper trust relationship, use the FQDN setup above and provide credentials for the proxy; also, if an error is received about not being able to connect, open PowerShell on the proxy and run Enable-PSRemoting) for the proxy server and click “Add”, then click “Next”.
    - Select “Create a group Managed Service Account” and fill in the “Enterprise Admin” username and password for the domain again and click “Next”.
    - Select the Azure AD domain with which you want to federate from the “DOMAIN” drop-down and select “Next”.
    - Under “Ready to configure”, leave defaults and select “Install” unless this is a production environment, and then consider selecting “Enable staging mode”.
    - On the “Configuration complete” screen if no errors, click “Next”.
    - Create an internal DNS record that points your AD FS farm FQDN to the internal IP address of your AD FS farm for internal users and also an external DNS record that points to the external IP address for your “Web Application Proxy” then select both checkboxes and click “Verify”.
    - If there are no errors, click “Exit”.
    - Manually validate by browsing to https://<ADFSFQDN>/adfs/fs/federationserverservice.asmx from inside the network and from the proxy which should display an XML document.
  - Publish the proxy pass-through application by opening “Remote Access Management” from the start menu and selecting the proxy name and then “Publish” in the right-hand menu.
    - Select “Next”.
    - Select “Pass-through” and “Next”
    - Enter a “Name:” (e.g., ADFS), “External URL:” (FQDN of AD FS farm; e.g., https://adfs.domain.com), select the appropriate certificate under “External certificate:” drop-down, and click “Next”.
    - Click “Publish”.
    - Then, validate outside the network by browsing to https://<ADFSFQDN>/adfs/ls where you should get a page with an error message.
    - If the request times out, ensure the Windows Firewall allows HTTP & HTTPS or ports 80 & 443 Inbound.
    - Finally, test the federation by going to https://portal.azure.com and logging in with one of the accounts for the federated domain. If you do not get redirected to your AD FS FQDN for login after entering the username, there may have been an error during the automated setup. If this is the case (DO NOT PERFORM THE FOLLOWING STEPS IF YOU ARE REDIRECTED), try the following:
      - Re-open Azure AD Connect, select “Configure”, then “Manage federation”, then “Next”.
      - On the “Manage federation” screen, select “Federate Azure AD domain”, then “Next”.
      - Enter the password for your Azure AD “Hybrid Identity/Global Administrator” and then click “Next”. You may have to re-enter the credentials in a pop-up window after clicking “Next”.
      - On the “Connect to AD FS” screen enter administrator credentials for your AD FS farm.
      - On the “Azure AD domain” screen, select the domain you wish to federate, validate the information, and click “Next”.
      - On the “Azure AD trust” screen, take note of changes it will make on your behalf and click “Next”.
      - Finally, click “Configure”.
    - Retry connecting to https://portal.azure.com to ensure it redirects to a federated user account.
  - If you used the Azure Deployment above, change the “Web Application Proxy” servers to point to the load balancer IP address for the AD FS farm (e.g., 10.0.0.200).
  - Configure Secondary AD FS Server and Web Application Proxy servers
**Option B** – Assisted setup with existing Azure AD Connect installation
  - Open “Azure AD Connect” on the synchronization server and click “Configure”.
  - Click “Manage federation”, then “Next”.
  - Click “Managed servers”, then “Next”.
  - Select the appropriate option for deploying either a server, a proxy, or connecting to a current AD FS farm and click “Next”
  - For an AD FS server
    - On the “Connect to AD FS” screen, enter administrator credentials for your AD FS farm.
    - On the “Specify SSL certificate” screen, browse to a certificate file for the domain you will be using for your AD FS farm FQDN and enter the password for the PFX file you select (see certificate requirements here learn.microsoft.com/en-us/windows-server/identity/ad-fs/design/certificate-requirements-for-federation-servers). If you are adding a secondary server, just enter the password for you existing certificate.
    - Select the appropriate “SUBJECT NAME” from the certificate for your AD FS form and then enter the prefix you want to assign to your farm to ensure it matches the “SUBJECT NAME” defined in the certificate (unless using a wildcard certificate in which case the prefix you wish to use for the farm; e.g., adfs, sso, sts) and click “Next” (these may already be selected for you if this is a secondary server).
    - Enter the fully-qualified server name for the AD FS server and click “Add”, then click “Next”.
    - Click “Configure”.
  - For a Web Application Proxy
    - Enable PS-Remoting on the Azure AD Connect Server and the Proxy by running the following commands from an elevated PowerShell session.
      - On the proxy, run “Enable-PSRemoting -force” then “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <AADConnectServerFQDN>” replacing <AADConnectServerFQDN> with the FQDN for the Azure AD Connect server and confirming with “Y” when prompted, then, “Restart-service -name winrm”.
      - On the AD connect server, run “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <DMZServerHostname> -Force -Concatenate”, run “Set-Item WSMan:\localhost\Client\TrustedHosts -Value <DMZServerIPAddress> -Force -Concatenate”, then “Restart-service -name winrm”.
      - Either change the internal facing network adapter for the proxy to “Private” or add firewall rules to allow WinRM from the Azure AD Connect server.
    - On the “Connect to AD FS” screen, enter administrator credentials for your AD FS farm.
    - On the “Specify SSL certificate” screen, browse to a certificate file for the domain you will be using for your AD FS farm FQDN and enter the password for the PFX file you select (see certificate requirements here learn.microsoft.com/en-us/windows-server/identity/ad-fs/design/certificate-requirements-for-federation-servers). If you are adding a secondary server, just enter the password for you existing certificate.
    - Select the appropriate “SUBJECT NAME” from the certificate for your AD FS form and then enter the prefix you want to assign to your farm to ensure it matches the “SUBJECT NAME” defined in the certificate (unless using a wildcard certificate in which case the prefix you wish to use for the farm; e.g., adfs, sso, sts) and click “Next” (these may already be selected for you if this is a secondary server).
    - Enter the Hostname or IP address (if the Proxy server is not joined to a domain with a proper trust relationship, use the FQDN setup above and provide credentials for the proxy; also if an error is received about not being able to connect open PowerShell on the proxy and run Enable-PSRemoting) for the proxy server and click “Add”, then click “Next”.
    - After validation, click “Configure”.
  - Once all servers are set up and configured, open “Azure AD Connect” again and click “Configure”.
  - Click “Manage federation”.
  - Click “Federate Azure AD domain”.
  - Enter the username and password for an Azure AD user with both the “Hybrid Identity & Global Administrator” roles and then click “Next”. You may have to re-enter the credentials in a pop-up window after clicking “Next”.
  - On the “Connect to AD FS” screen enter administrator credentials for your AD FS farm.
  - On the “Azure AD domain” screen, select the domain you wish to federate, validate the information, and click “Next”.
  - On the “Azure AD trust” screen, take note of changes it will make on your behalf and click “Next”.
  - Finally, click “Configure”.
After the domain is federated with AD FS, enable NL3 Account Protection using the Microsoft AD FS Risk Assessment Model Plugin and sample code provided by NL3.

## Install Microsoft AD FS Risk Assessment Model Plugin for the Account Protection Check.
  - Follow the instructions in the following GitHub repository in the README.md file:
    - [https://github.com/Next-Level3/nl3-adfs-plugin](https://github.com/Next-Level3/nl3-adfs-plugin)
    - When filling out the appConfig.csv file, use “urn:federation:Microsoft” without quotes for the LookupKey associated with the AppName and SigningKey you create in the Next Level3 Company Portal for Microsoft Online services and applications.
    - For steps on setting up your application and generating and validating your signing key for Microsoft Online services, please see <placeholder>.
  - Enable users for account protection in the Next Level3 Company Portal. Make sure to use the fully qualified user account (e.g., <username>@<subdomain>.<domain>.com).
_**It may be possible to use other SAML Identity Providers if the SAMLp protocol is supported.**_
  
## Instructions for Third-party SAMLp providers
While the following instructions favor AD FS, they should work with any identity provider that supports SAMLp which has differences from the regular SAML protocol. Compatible identity providers will support “SAML 2.0 compliant SP-Lite profile-based Identity Provider” standards.

[https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-saml-idp](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-fed-saml-idp)
```javascript
const https = require("https");
const nJwt = require("njwt");

function getLockStatus(jwt, apiHost, apiPath, requestHeaders) {
  return new Promise((resolve, reject) => {
    const postData = JSON.stringify({
      userIP: requestHeaders["x-forwarded-for"],
      userDevice: requestHeaders["user-agent"],
      userLocation: "",
      integrationType: "aadb2c",
      integrationData: {},
    });
    const options = {
      host: apiHost,
      port: "443",
      path: apiPath,
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
        "x-nl3-authorization-token": jwt,
        "Content-Length": Buffer.byteLength(postData),
      },
    };

    const req = https.request(options, (response) => {
      const chunksOfData = [];

      response.on("data", (fragments) => {
        chunksOfData.push(fragments);
      });

      response.on("end", () => {
        const responseBody = Buffer.concat(chunksOfData);
        resolve(responseBody.toString());
      });
        
      response.on("error", (error) => {
        console.log(`Error = ${error.message}`);
        reject(error);
      });
    });
    req.write(postData);
            req.end();
  });
}

module.exports = async function (context, req) {
  let responseMessage = "";
  let responseStatus = 200;
  const claims = {
    iss: process.env.APP_URI,
    aud: process.env.API_HOST,
    sub: req.body.signInName,
  };
  /* Ideally this Signing Key would be stored and retrieved from a secrets manager
     and not an environmental variable */
  const decodedDomainToken = Buffer.from(process.env.SIGNING_KEY, "base64");
  const jwt = nJwt.create(claims, decodedDomainToken);
  jwt.setExpiration(new Date().getTime() + 60 * 5 * 1000); // 5 minute expiration
  jwt.setNotBefore(new Date().getTime() - 60 * 1 * 1000); // 1 minute leeway
  const authToken = jwt.compact();

  const res = await getLockStatus(
    authToken,
    process.env.API_HOST,
    process.env.API_PATH,
    req.headers
  );
  const result = JSON.parse(res);
  let failed = false;

  if (result) {
    context.log(JSON.stringify(result));
    if (Object.prototype.hasOwnProperty.call(result, "locked")) {
      if (result.locked) {
responseStatus = 409;
responseMessage = process.env.LOCKED_MESSAGE;
      }
    } else {
      failed = true;
    }
  } else {
    failed = true;
  }
  if (failed) {
    if (process.env.FAIL_CLOSED == "true") {
      responseStatus = 409;
      responseMessage = "NL3 Account Protection Check failed and configuration is set to fail closed!";
    }
  }
  context.res = {
    status: responseStatus,
    body: responseMessage
  };
};
```
The environmental variables for API_HOST, API_PATH, APP_URI, FAIL_CLOSED, LOCKED_MESSAGE, and SIGNING_KEY can be set by opening your Function App in the Azure Portal, then clicking on ‘Configuration’ and adding each as a ‘New application setting’ under ‘Application settings’. We have added all values as environmental variables for simplicity, but it is recommended that the ‘SIGNING_KEY’ be stored in a secrets manager like Azure Key Vault instead of being stored as an environmental variable when possible which will require some minor updates to the code (please contact support for guidance). The URL to use for your ‘ServiceUrl’ in custom policy can be found in your ‘Function App’ under ‘Functions’, then click on your function’s name, then click on the ‘Get Function Url’ selection at the top (you can remove the ?code= . . . parameter at the end since we will be providing that code in the headers).

Once your function is created and you have updated the ‘ServiceUrl’ in your custom policy, you will need to deploy the custom policy in Azure. If you have already set up a custom policy to support your application previously, you will only need to upload the modified ‘TrustFrameworkBase.xml’ policy. However, if you have not previously leveraged custom policy, guidance can be found here on how to set up the pre-requisites and upload a policy:

[https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy)

Then, depending on the type of application you are integrating, you will need to update the corresponding settings to point to the custom policy. Examples are provided for a variety of application types listed under ‘Next Steps’ in the above-referenced tutorial.

*If you use IAM Roles and not IAM Users, please contact support to discuss the options for implementation and specific design considerations that may be required to implement this correctly using roles.

The next step is to setup the Amazon EventBridge event rule that triggers the Lambda function for specific events. Here is a sample rule:

References:

https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html
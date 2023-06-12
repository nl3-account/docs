## Microsoft Azure AD B2C Integration
<img src="/images/microsoft b2c/azure1.png" />
The Next Level3 Azure AD B2C integration is designed to be used for any of your existing applications or sites which are using Azure AD for authentication. This integration will allow you to add Account Protection for any application which leverages Azure AD B2C for authentication. 

Pre-requisites

  * Application leveraging Azure AD B2C for authentication
  * Next Level3 Company Account
  * Signing Key created for an application in the Next Level3 Company Portal

### Enabling NEXT LEVEL3 AZURE AD B2C Custom Policy to SIGN IN
The first step to add an NL3 Account Protection Check to an existing application that is using Azure AD B2C for authentication is to create a custom policy to add the check to your existing login flow. Unless you are very familiar with Azure AD B2C policies and user journeys, we recommend downloading the starter packs from here: GitHub project.

For most of you, assuming you have already implemented Azure AD B2C, you may already be familiar with the policies you are using and you can start with those policies.

For this integration, we used the standard ‘Local Accounts’ starter pack downloaded from the GitHub repository referenced above. The only change we made to the policies listed in the ‘Local Accounts’ folder of that repo was in the TrustFrameworkBase.xml policy. We added the following ‘ClaimsProvider’ to line 453 underneath the existing ‘Local Account SignIn’ ‘ClaimsProvider’:
    ```Markup
    <ClaimsProvider>
      <DisplayName>Local Account NL3 Protection Check</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="REST-NL3AccountProtectionCheck">
          <DisplayName>Perform NL3 Account Protection Check</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
              <Item Key="ServiceUrl">https://[your-service-endpoint].azurewebsites.net/api/AccountProtectionCheck</Item>
              <Item Key="AuthenticationType">ApiKeyHeader</Item>
              <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <CryptographicKeys>
              <Key Id="x-functions-key" StorageReferenceId="B2C_1A_RestApiKey" />
          </CryptographicKeys>
          <InputClaims>
              <InputClaim ClaimTypeReferenceId="signInName" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```
 The ‘ServiceUrl’ points to an API endpoint that runs the code for performing the NL3 Account Protection Check. We have deployed this as an Azure Function, but as long as it is a RESTful API endpoint that validates the API key in the headers, performs the Protection Check, and returns the proper status codes and messages, the technology used is not important. We will use an Azure Function for this example. If you are not familiar with Azure Functions, the following guides can be helpful:
[https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-app-portal](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-app-portal)

[https://docs.microsoft.com/en-us/azure/azure-functions/create-first-function-cli-csharp?tabs=azure-cli%2Cin-process](https://docs.microsoft.com/en-us/azure/azure-functions/create-first-function-cli-csharp?tabs=azure-cli%2Cin-process)

[https://docs.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-node](https://docs.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-node)

The function in this example uses the Node.js 16 runtime and this is the code you can use within your function to perform the protection check:

<Tip>If you use IAM Roles and not IAM Users, please contact support to discuss the options for implementation and specific design considerations that may be required to implement this correctly using roles.</Tip>

The next step is to setup the Amazon EventBridge event rule that triggers the Lambda function for specific events. Here is a sample rule:

References:

[https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html)
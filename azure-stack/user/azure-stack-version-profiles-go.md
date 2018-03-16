---
title: Using API version profiles with GO in Azure Stack | Microsoft Docs
description: Learn about using API version profiles with GO in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''

ms.assetid: 1F335E3A-37B0-4D6D-A77E-DCBF8D21EF73
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg

---

# Use API version profiles with Go in Azure Stack

*Applies to: Azure Stack integrated systems and Azure Stack Development Kit*

## Go and profiles

A profile is a combination of different resource types with different versions from different services. Using a profile will help you mix and match between various resource types. Profiles can provide:

 - Stability for your application by locking to specific API versions.
 - Compatibility for your application with Azure Stack and regional Azure datacenters.

In the Go SDK, profiles are available under the profiles/ path, with their version in the **YYYY-MM-DD** format. Right now, the latest Azure Stack profile version is **2017-03-09**. To import a given service from a profile, you need to import its corresponding module from the profile. For example, to import **Compute** service from **2017-03-09** profile:

````go
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/compute/mgmt/compute" 
````

## Install Azure SDK for Go

  1. Install Git. For instructions, see [Getting Started - Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
  2. Install the [Go Programming Language](https://golang.org/dl).  
  Note that to use profiles you need to install Go version 1.9 or newer.
  3. Install the Go Azure SDK and its dependencies by running the following bash command:
  ````go
    go get -u -d github.com/Azure/azure-sdk-for-go/...
  ````
You can find information about the Azure Go SDK at [Installing the Azure SDK for Go](https://docs.microsoft.com/go/azure/azure-sdk-go-install). The Azure Go SDK is publicly available at the [Go SDK on GitHub](https://github.com/Azure/azure-sdk-for-go).

The GO SDK depends on Go-AutoRest modules to send REST requests to Azure Resource Manager endpoints. You will need to import the Go-AutoRest module dependencies from [Go-AutoRest on GitHub](https://github.com/Azure/go-autorest).


## How to use GO SDK profiles on Azure Stack

To run a sample of Go code on Azure Stack:
  1. Install Azure SDK for Go and its dependencies (see previous section)
  2. Get metadata information from resource manager endpoint; which will return a JSON file with required information to run Go samples.
  Note: ResourceManagerUrl in one node environment is: https://management.local.azurestack.external/ 
     ResourceManagerUrl in multi node environment is: https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/ To retrieve the metadata required: <ResourceManagerUrl>/metadata/endpoints?api-version=1.0
  Sample JSON file:

```json
{ " gal int " 
"https : / /portal . local . azurestack. external : 30015/" 
"graphEndpoint" • "https: / / graph. windows . net/ " 
"portal Endpoint " 
"https : / / portal . local . azurestack. external / " 
" authentication": {
" loginEndpoint " 
"https: //login. windows . net/" 
" audiences" 
[ "https : / / management . azurestackci04 . onmicrosoft . com/3eeB99cB 
-c137-4ce4-b230-619961a09a73"] 
}
}
```
  
  
  3. If not available, create a subscription and save the subscription ID to be used later.Instructions to create a subscription are here. 
  4. Create a service principal with "Subscription" scope and "Owner" role. Save the service principal's ID and secret.Instructions to create a service principal for Azure Stack are here.  
  5. Now your Azure Stack environment is setup. In your code, import a service module from Go SDK profile. The current version of Azure Stack profile is "2017-03-09". For example, to import network module from "2017-03-09" profile: 

````go
package main 
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/network/mgmt/network"
````
  
  6. In your function, create and authenticate a client with a New*Client function call. For example to create a virtual network client:    
  
````go
package main 
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/network/mgmt/network" 
func main() { 
vnetC1ient : 
network. NewVirtua1NetworksC1ientWithBaseURI( " <baseURI>", "(subscriptionID>") 
vnetC1ient .Authorizer = autorest. NewBearerAuthorizer(token) 

````
  
  Set <baseURI> to the "ResourceManagerUrl" value used in step #2.Set <subscriptionID> to the SubscriptionID value saved from step #3.
  To create token see Authentication section below.  
  
  7. Invoke API methods by using the client that you created in the previous step. For example, to create a virtual network by using our client from previous step: 
  
````go
package main 
import "github.com/Azure/azure-sdk-for-go/profi1es/2e17-e3-eg/network/mgmt/network" 
func main() { 
vnetC1ient : 
network. NewVirtua1NetworksC1ientWithBaseURI( " "(subscriptionID>") 
vnetC1ient .Authorizer = autorest. NewBearerAuthorizer(token) 
vnetC1ient .CreateOrUpdate( ) 

````
  
  For a complete example of creating a virtual network on Azure Stack by using Go SDK profile, see Example section below.

## Authentication

To get the Authorizer property from Azure Active Directory using Go SDK, install the Go-AutoRest modules. These modules should have been already installed with the "Go SDK" installation; if not, install the authentication package. 
The Authorizer must be set as the authorizer for the resource client. There are different methods to get an Authorizer; for a complete list see here. 
This section presents a common way to get authorizer tokens on Azure Stack by using client credentials:
  1. If a service principal with owner role on the subscription is available, skip this step. Otherwise create a service principal (instructions) and assign it an "owner" role scoped to your subscription (instructions). Save the service principal application ID and secret.  
  2. Import adal package from Go-AutoRest in your code. 
  
````go
package main 
import "github.com/Azure/go-autorest/autorest/adal" 

````
  
  3. Create an oauthConfig by using NewOAuthConfig method from adal module. 
  
````go
package main 
import "github.com/Azure/go-autorest/autorest/ada1" 
func CreateToken() (adal. OAuthTokenProvider, error) { 
var token adal .0AuthTokenProvider 
oauthConfig, err : 
adal. NewOAuthConfig(activeDirectoryEndpoint, 
tenantlD) 

````
   
  Set <activeDirectoryEndpoint> to the value of "loginEndpoint" property from the ResourceManagerUrl metadata retrieved on the previous section of this document.
  Set <tenantID> value to your Azure Stack tenant ID. 
  4. Finally, create a service principal token by using NewServicePrincipalToken method from adal module. 

````go
package main 
import "github.com/Azure/go-autorest/autorest/adal" 
func CreateToken() (adal. OAuthTokenProvider, error) { 
var token adal .0AuthTokenProvider 
oauthConfig, err 
adal. NewOAuthConfig(activeDirectoryEndpoint, 
token, err = adal. NewservicePrincipa1Token( 
*oauthConfig, 
clientlD, 
clientSecret, 
activeDirectoryResourceID) 
return token, err 
tenantlD) 
````
  
  Set <activeDirectoryResourceID> to one of the values in the "audience" list from the ResourceManagerUrl metadata retrieved on the previous section of this document.
  Set <clientID> to the service principal application ID saved when service principal was created on the previous section of this document.
Set <clientSecret> to the service principal application Secret saved when service principal was created on the previous section of this document.

## Example

This section shows a sample of Go code to create virtual network on Azure Stack. For complete examples of Go SDK see Azure Go SDk samples repository.  Azure stack samples are available under hybrid/ path inside service folders of the repository.
Note: to run the code in this example, verify that the subscription used has "Network" resource provider listed as "Registered." To verify it, look for the Subscription in the Azure Stack portal, and click on "Resource providers."

  1. Import required packages in your code. You should use the latest available profile on Azure Stack to import the network module. 
  
````go
package main 
import ( 
" context" 
..fmt" 
"github.com/Azure/azure-sdk-for-go/profiles/2817-03-eg[network/mgmt/network" 
"github.com/Azure/go-autorest/autorest" 
"github.com/Azure/go-autorest/autorest/adal" 
"github.com/Azure/go-autorest/autorest/to" 

````
  
  2. Define your environment variables. Note that to create a virtual network you need to have a resource group. 
  
````go
package main 
import ( 
" context" 
..fmt" 
"github.com/Azure/azure-sdk-for-go/profiles/2817-03-eg[network/mgmt/network" 
"github.com/Azure/go-autorest/autorest" 
"github.com/Azure/go-autorest/autorest/adal" 
"github.com/Azure/go-autorest/autorest/to" 

````
  
  3. Now that you have defined your environment variables, add a method to create authentication token by using adal package. See details about authentication in previous section. 
  
````go
//CreateToken creates a service princi pal token 
func CreateToken() (adal. OAuthTokenProvider, error) { 
var token adal .0AuthTokenProvider 
oauthConfig, err 
adal. NewOAuthConfig( activeDirectoryEndpoint, 
token, err = adal. NewservicePrincipa1Token( 
*oauthConfig, 
clientlD, 
clientSecret, 
activeDirectoryResourceID) 
return token, err 
tenantlD) 

````
  
  4. Add the main method. The main method first gets a token by using the method that is defined in previous step. Then, it creates a client by using network module from profile. Finally, it creates a virtual network. 
  
````go
package main 
import ( 
" context" 
..fmt" 
"github.com/Azure/azure-sdk-for-go/profiles/2817-03-eg[network/mgmt/network" 
"github.com/Azure/go-autorest/autorest" 
"github.com/Azure/go-autorest/autorest/adal" 
"github.com/Azure/go-autorest/autorest/to" 
var ( 
activeDirectoryEndpoint 
tenantlD 
clientlD 
clientSecret 
activeDi rectoryResourceID 
subscriptionID 
baseURI 
resourceGroupName 
"your Login Endpoi ntFromResourceManagerUr1Metadata" 
"yourAzureStackTenantID" 
"yourservi cePrincipa1App1icationID" 
"yourServicePrincipa1Secret" 
" yourAudi enc e From Resour cemanagerUr1metadata " 
"yourSubscriptionID" 
"yourResourcemanagerURL" 
"existingResourceGroupName" 
//CreateToken creates a service princi pal token 
func CreateToken() (adal. OAuthTokenProvider, error) { 
var token adal .0AuthTokenProvider 
oauthConfig, err 
adal. NewOAuthConfig(activeDirectoryEndpoint, 
token, err = adal. NewservicePrincipa1Token( 
*oauthConfig, 
clientlD, 
clientSecret, 
activeDirectoryResourceID) 
return token, err 
func main() { 
token , 
CreateToken() 
vnetC1ient : 
network. Newvi rtua INetwor ksC1ie ntWi thBaseURI (baseURI , 
vnetC1ient .Authorizer = autorest. NewBearerAuthorizer(token) 
tenantlD) 
subscriptionID) 
future, 
vnetC1ient. CreateOrUpdate( 
context. Background() , 
reso urceGroupName , 
" sampleVnetName " , 
network. Vi rtua1Network{ 
Location: to. 
VirtualNetworkPropertiesFormat: &network. VirtualNetworkPropertiesFormat{ 
AddressSpace: &network.AddressSpace{ 
AddressPrefixes: 
Subnets: Subnet{ 
Name: to. StringPtr("subnetName"), 
SubnetPropertiesFormat: &network .SubnetPropertiesFormat{ 
AddressPrefix: 
future. WaitForComp1etion(context. Background ( ) , 
err • 
if err 
nil { 
fmt. Printf (err. Error()) 
return 
vnetC1 ient. Client) 
````


## Next steps
* [Install PowerShell for Azure Stack](azure-stack-powershell-install.md)
* [Configure the Azure Stack user's PowerShell environment](azure-stack-powershell-configure-user.md)  

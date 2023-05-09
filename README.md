# [Spring PetClinic Sample Application](https://github.com/spring-projects/spring-petclinic) using a full set of Azure solutions

* [Azure AppService](https://azure.microsoft.com/en-us/products/app-service/) for app hosting,
* [Azure Database for MySQL](https://azure.microsoft.com/en-us/products/mysql/) for storage,
* [Azure Monitor](https://azure.microsoft.com/en-us/products/monitor/)([Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview?tabs=net)) for monitoring and logging.
* [Key Vault](https://azure.microsoft.com/en-us/products/key-vault/) for securing secret/password.

## Prerequisites
* Java 17 or later
* Azure Deployment Environment has provisioned the environment

## Quickstart
1, Clone this repo
2, Use VS Code to open the cloned folder
3, Right click the blank space in the VS Code Explorer and select "Deploy to Web App...". (Please install "Azure Tools" if no such option)
4, Select subscription and the App Service provisioned

How to Visit:
Go to the App service overview in Azure Portal, click the Default domain, There will be petlicnic page.

## Run application locally

> NOTE: Azure Database for MySQL flexible servers don't allow connections from local machines by default for security. 
> You must add current IP address of your local machine to the firewall rules in [Azure Portal](https://ms.portal.azure.com/#view/HubsExtension/BrowseResource/resourceType/Microsoft.DBforMySQL%2Fservers) before running the application locally.

![add firewall rule to allow local connections](readme.assests/add_mysql_firewall_rule.png)

### VSCode
You can just launch the predefined `Debug PetClinic` configuration to run the application locally if you are using VSCode. 

![run application locally in VSCode](readme.assests/run_locally_vscode.png)

### IntelliJ IDEA
You need to pass the environment variables below to the application first. Values of these
environment variables are all available in the `.azure/${Environment-Name}/.env` file if `azd provision` or `azd up ...` completes successfully.

![setup environment variables in IntelliJ IDEA](readme.assests/run_locally_intellij.png)

```properties
# activate `azure` and `mysql` spring profiles
SPRING_PROFILES_ACTIVE=azure,mysql
# Azure Application Insights connection string, for monitoring and logging
APPLICATIONINSIGHTS_CONNECTION_STRING=...
# Azure Key Vault endpoint, where the MySQL user password (${MYSQL_PASS}) is stored
AZURE_KEY_VAULT_ENDPOINT=...
# Azure Database for MySQL server jdbc url
MYSQL_URL=...
# Azure Database for MySQL server user name
MYSQL_USER=...
```

## Security

### Roles

This template creates a [managed identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
for your app inside your Azure Active Directory tenant, and it is used to authenticate your app with Azure and other services
that support Azure AD authentication like Key Vault via access policies. You will see principalId referenced in the infrastructure
as code files, that refers to the id of the currently logged in Azure CLI user, which will be granted access policies and permissions
to run the application locally. To view your managed identity in the Azure Portal, follow these
[steps](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-view-managed-identity-service-principal-portal).

### Key Vault

This template uses [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/overview) to securely store user password
for the provisioned Azure Database for MySQL flexible server. Key Vault is a cloud service for securely storing and accessing secrets
(API keys, passwords, certificates, cryptographic keys) and makes it simple to give other Azure services access to them. As you
continue developing your solution, you may add as many secrets to your Key Vault as you require.


## Credits

This Spring microservices sample is forked from
[Azure-Samples/spring-petclinic-java-mysql](https://github.com/Azure-Samples/spring-petclinic-java-mysql).


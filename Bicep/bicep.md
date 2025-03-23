# Bicep Hackathon

An open source version of [Hack with Upperground](https://www.eventbrite.com.au/e/hack-with-upperground-bicep-tickets-1252403756349), so you can try out the challenges in your own subscription.

## Table of Contents

- [Challenge 1 - Set-up Your Environment](#challenge-1---set-up-your-environment)
- [Challenge 2 - Deploy Your First Bicep Resource](#challenge-2---deploy-your-first-bicep-resource)
- [Challenge 3 - Make Your Bicep File Reusable](#challenge-3---make-your-bicep-file-reusable)
- [Challenge 4 - Use Expressions](#challenge-4---use-expressions)
- [Challenge 5 - What If and Deployment Modes](#challenge-5---what-if-and-deployment-modes)
- [Challenge 6 - Decorating Parameters, and Interpolation](#challenge-6---decorating-parameters-and-interpolation)
- [Challenge 7 - Deploying Multiple Resources with Loops and Batching](#challenge-7---deploying-multiple-resources-with-loops-and-batching)
- [Challenge 8 - Symbolic Names and Outputs](#challenge-8---symbolic-names-and-outputs)
- [Challenge 9 - Conditional Deployments](#challenge-9---conditional-deployments)
- [Challenge 10 - Working with Existing Resources and Scopes](#challenge-10---working-with-existing-resources-and-scopes)
- [Challenge 11 - Advanced Parameters and User-Defined Data Types](#challenge-11---advanced-parameters-and-user-defined-data-types)
- [Challenge 12 - Linting and Testing](#challenge-12---linting-and-testing)
- [Challenge 13 - Convert Existing Resources](#challenge-13---convert-existing-resources)
- [Challenge 14 - Azure Verified Modules](#challenge-14---azure-verified-modules)


## Challenge 1 - Set-up Your Environment

### Objectives

1. In this challenge you will install the tools required to work with Bicep.
1. Connect to the Virtual Machine using the Session Details. You can use your own Remote Desktop client with the IP Address, or use Remote Desktop in this window or a separate window using the buttons above.
1. Install either Azure PowerShell or the Azure CLI
    - Take your pick, depending on your preference. You will need to authenticate and deploy Azure resources
1. Install the Bicep CLI
    - Ensure Bicep can be executed from any directory, without specifying a full path.
1. Install the Bicep Visual Studio Code Extension
    - Subtitle Ensure you can use the Bicep extension in Visual Studio Code.

### Tips

- **Command-line Tools**
    - Chocolately has been pre-installed on your VM, so you can quickly install Terraform with chocolatey.
        - [Install Azure CLI with Chocolatey](https://community.chocolatey.org/packages/azure-cli)
        - [Install Azure PowerShell with Chocolatey](https://community.chocolatey.org/packages/az.powershell)
        - [Install Bicep CLI with Chocolatey](https://community.chocolatey.org/packages/bicep)
    - Alternatively, you can install Bicep using either Azure PowerShell or the Azure CLI, or manually.
        - [Install Bicep Tools](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/install)
- **Visual Studio Code Extensions**
    - Visual Studio Marketplace: [Bicep](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)  

### Possible Solutions

```powershell
choco install azure-cli -y
choco install bicep -y
code --install-extension ms-azuretools.vscode-bicep
```

## Challenge 2 - Deploy Your First Bicep Resource

### Objectives

1. In this challenge you will create a Bicep file and deploy a storage account using Bicep.
1. Create a main.bicep file in C:\Bicep.
1. In C:\Bicep\main.bicep, create a Storage Account resource with the following properties:- 
    - Name: Any valid, globally unique name
    - Location: australiaeast
    - Kind: StorageV2
    - SKU Name: Standard_LRS

1. The VM has a Managed Identity assigned, so you can authenticate using <code>Connect-AzAccount -Identity</code> OR <code>az login --identity</code>
1. Deploy the storage account to the <code>$LabResourceGroupName</code> resource group using either the Azure CLI or PowerShell.

### Tips

- **Bicep Documentation**
    - [Bicep Resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/file#resources)
    - [Microsoft.Storage/storageAccounts](https://learn.microsoft.com/en-us/azure/templates/microsoft.storage/storageaccounts?pivots=deployment-language-bicep)
- **Azure PowerShell Documentation**
    - [Connect-AzAccount](https://learn.microsoft.com/en-us/powershell/module/az.accounts/connect-azaccount)
    - [New-AzResourceGroupDeployment](https://learn.microsoft.com/en-us/powershell/module/az.resources/new-azresourcegroupdeployment)
- **Azure CLI Documentation**
    - [az login](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli-interactively)
    - [az deployment group create](https://learn.microsoft.com/en-us/cli/azure/deployment/group?view=azure-cli-latest#az-deployment-group-create)

### Possible Solutions

```powershell
Connect-AzAccount -Identity

New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep"

```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/2)

## Challenge 3 - Make Your Bicep File Reusable

### Objectives

1. In this challenge you will make your Bicep file resuable by adding parameters.
2. Add the following parameters to your Bicep File:

    - name - Must be unique globally. Do not set a default value
    - location - Set a default value to australiaeast
    - skuName - Set a default value to Standard_LRS

1. Deploy a second storage account. Pass the parameters using the command line at deployment time. Set the skuName to <code>Standard_ZRS</code>, use the default location and any valid name.
1. Deploy a third storage account. Pass the parameters using a Bicep Parameter file (Not JSON) named <code>main.bicepparam</code> in C:\Bicep. Set the skuName to <code>Standard_GRS</code>.

### Tips

- [Parameters in Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/parameters)
- [Resource Name Rules](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/resource-name-rules)
- [Bicep Scope Functions resourceGroup](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-functions-scope#resourcegroup)
- [Microsoft.Storage/storageaccounts.sku.name](https://learn.microsoft.com/en-us/azure/templates/microsoft.storage/storageaccounts?pivots=deployment-language-bicep#sku)
- [Create parameters files for Bicep deployment](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/parameter-files?tabs=Bicep)
- [Deploy with Azure CLI](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-cli)
- [Deploy with Azure PowerShell](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/deploy-powershell)

### Possible Solutions

```powershell
Connect-AzAccount -Identity

## Deploy a second storage account, using parameters at the command line
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -skuName 'Standard_ZRS' -nameFromTemplate "st$(Get-Random -Minimum 111111 -Maximum 999999)"

## Deploy a third storage account, using parameters in main.bicepparam
Start-BitsTransfer -Source 'https://raw.githubusercontent.com/waynehoggett/HackathonSetup/refs/heads/main/bicep/solutions/3/main.bicepparam' -Destination "C:\Bicep\main.bicepparam"
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -TemplateParameterFile "C:\Bicep\main.bicepparam"
```


- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/3)

## Challenge 4 - Use Expressions

### Objectives

1. In this challenge you will use expressions to expressions to simplify and improve your Bicep file.
1. Use an expression to set the default value of the location parameter to the location (region) of the resource group the resources are deployed to. This can be achieved with a scope function. 
1. Storage account names must be globally unique. If a storage account name parameter value is not provided by the user, generate a unique deterministic value. When you redeploy your Bicep file with the same parameters, resources should maintain the same names. 
1. Storage account names must always be lower case, use an expression to make sure any parameter values provided for the storage account name are automatically converted to lower-case. 
1. Deploy your Bicep file with the updated expressions. You should not need to provide any parameters at deployment time. 

### Tips

- [Bicep scope functions](https://learn.microsoft.com/azure/azure-resource-manager/bicep/bicep-functions#scope-functions)
- [Bicep string functions](https://learn.microsoft.com/azure/azure-resource-manager/bicep/bicep-functions#string-functions)

### Possible Solutions

```powershell
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -WhatIf
```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/4)

## Challenge 5 - What If and Deployment Modes

### Objectives

1. In this challenge you will explore What If and Deployment Modes. Deployment Modes and WhatIf can be used in a production enviroment to assist with code review, when submitting pull requests.
1. Important: Do these steps in the order shown.
1. Using the default (Incremental) deployment mode. Preview changes programatically using WhatIf, without providing any parameters via the command-line or file. Write the output to <code>C:\Output\whatif1.txt</code>.
1. Using the Complete deployment mode, Preview changes programmatically using WhatIf again, write the output to <code>C:\Output\whatif2.txt</code>.
1. Using the Complete deployment mode, deploy your template without any parameters.

### Tips

- [ARM template deployment what-if operation: What-if commands](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-what-if?tabs=azure-powershell#what-if-commands)
- [Azure Resource Manager deployment modes](https://learn.microsoft.com/azure/azure-resource-manager/templates/deployment-modes)


### Possible Solutions

```powershell
## WhatIf Incremental
Get-AzResourceGroupDeploymentWhatIfResult -Mode Incremental -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" | Out-File "C:\Output\whatif1.txt"

## What If Complete
Get-AzResourceGroupDeploymentWhatIfResult -Mode Complete -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" | Out-File "C:\Output\whatif2.txt"

## Complete Deployment
New-AzResourceGroupDeployment -Mode Complete -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -Force

```


- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/5)

## Challenge 6 - Decorating Parameters, and Interpolation

### Challenges

1. In this challenge you will decorate parameters and use string interpolation.
1. Update the storage account SKU Name parameter so that only values accepted by Azure Resource Manager (ARM) Resource Provider are allowed. 
1. Change the storage account name parameter to automatically add the prefix `st`, the bicep file must provide a unique suffix, by default, for the storage account name, if the parameter value is not provided. 
1. Update the storage account name parameter so that it only allows a length that is allowed by the resource provider. You'll have to make sure the combined length of the prefix and your unique value does not exceed the length allowed.

### Tips

- [Bicep Parameters - Use decorators](https://learn.microsoft.com/azure/azure-resource-manager/bicep/parameters#use-decorators)
- [Bicep Data Types - Strings](https://learn.microsoft.com/azure/azure-resource-manager/bicep/data-types#strings)
- [Microsoft.Storage storageAccounts](https://learn.microsoft.com/azure/templates/microsoft.storage/storageaccounts?pivots=deployment-language-bicep)
- To use string interpolation - Refer to the documentation for Bicep Data Types - Strings documentation in the link above

### Possible Solutions

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/6)


## Challenge 7 - Deploying Multiple Resources with Loops and Batching

### Challenges

1. In this challenge you will deploy multiple resources with loops and batching.
1. Modify your Bicep file to include a parameter named count. Set the default value to 1. Set a maximum value of 9. Use a loop to deploy the number of storage accounts specified by count. 
1. Update your file to automatically generate names for the storage accounts. Unique names must begin with st and end with the current count, starting at 1, not zero. 
1. Instruct Bicep to deploy storage accounts in batches of 3. 
1. Deploy 3 storage accounts using your updated Bicep file, the storage account names must each start with st and end with 1, 2 and 3 respectively. 


### Tips

- [Iterative loops in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/loops)
- [Deploy in batches](https://learn.microsoft.com/azure/azure-resource-manager/bicep/loops#deploy-in-batches)


### Possible Solutions




- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/7)


## Challenge 8 - Symbolic Names and Outputs

### Challenges

- In this challenge you will update the bicep file to output values from the deployment.
- Create an output named <code>storageAccountNames</code> to output all the storage account names deployed by the template.
- Create an output named <code>blobServiceUris</code> to output the blob service URI for each storage account.
- Deploy 3 storage accounts using the default parameters and review the output.


### Tips

- [Outputs in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/outputs?tabs=azure-powershell)
- [Loop syntax](https://learn.microsoft.com/azure/azure-resource-manager/bicep/loops#loop-syntax)


### Possible Solutions

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/8)

## Challenge 9 - Conditional Deployments

### Challenges

1. In this challenge, you will perform conditional deployments. 
2. Add a parameter named <code>dataConfidentialityLevel</code> with three allowed values: <code>OFFICIAL</code>, <code>SENSITIVE</code>, and <code>PROTECTED</code>. Set the default value to <code>PROTECTED.</code>
3. Update main.bicep, so that if the value of dataConfidentialityLevel is PROTECTED, the storage accounts <code>allowBlobPublicAccess</code> property should be set to false, otherwise it should be set to null (Default).
4. Update main.bicep, so that if the value of <code>dataConfidentialityLevel</code> is <code>SENSITIVE</code> or <code>PROTECTED</code> the storage accounts <code>supportsHttpsTrafficOnly</code> property should be set to true, otherwise it should be set to null (Default).
5. Update main.bicep to a tag named <code>dataConfidentialityLevel</code> to the storage account with the value of the parameter <code>dataConfidentialityLevel</code> set in the tag.
6. Deploy one <code>SENSITIVE</code> storage account. 

### Tips

- [Conditional deployments in Bicep with the if expression](https://learn.microsoft.com/azure/azure-resource-manager/bicep/conditional-resource-deployment)
- [Microsoft.Storage/storageAccounts](https://learn.microsoft.com/en-us/azure/templates/microsoft.storage/storageaccounts?pivots=deployment-language-bicep)


### Possible Solutions

```powershell
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -Force -Count 1 -dataConfidentialityLevel "SENSITIVE"
```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/9)

## Challenge 10 - Working with Existing Resources and Scopes

### Challenges

1. In this challenge, you will work with existing resources and use scopes. Working with existing resources across multiple scopes is an essential skill in enterprise deployments.
1. Update main.bicep so that when a <code>SENSITIVE</code> storage account is deployed, <code>networkAcls</code> are added to permit access from only <code>Subnet1</code> in an existing VNet named <code>VNet1</code> in the resource group named <code>$VNetLabResourceGroupName</code>, denying access from other sources such as Azure and the public Internet. Do not hardcode Resource ID values, but you can hardcode resource names when referencing existing resources.
1. Deploy one <code>SENSITIVE</code> storage account with the updated specification.

### Tips

- [Existing resources in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/existing-resource)
- [Scopes (targetScope) - Resource Group](https://learn.microsoft.com/azure/azure-resource-manager/bicep/deploy-to-resource-group?tabs=azure-cli)
- Bicep Issue: [bicep error Values for request parameters are invalid: networkAcls.virtualNetworkRules\[*\].id](https://github.com/Azure/bicep-types-az/issues/1555)


### Possible Solutions

```powershell
$VNetResourceGroup = (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-vnet-*").ResourceGroupName
(Get-Content "C:\Bicep\main.bicep").Replace('%VNET_RESOURCE_GROUP%', "$($VNetResourceGroup)") | Set-Content "C:\Bicep\main.bicep"

New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -Force -Count 1 -dataConfidentialityLevel "SENSITIVE"

```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/10)


## Challenge 11 - Advanced Parameters and User-Defined Data Types

### Challenges

1. In this challenge you will explore advanced parameters and resource nesting.
2. Replace the existing count parameter with new logic that accepts multiple storage accounts, for which you can optionally specify the <code>name</code>, <code>dataConfidentialityLevel</code>, <code>location</code>, and <code>skuName</code>. Retain the existing defaults.

1. You can delete existing storage accounts if required.
1. Deploy 3 storage accounts with the following configurations:- 
    - **Storage Account 1:**
        - dataConfidentialityLevel: OFFICIAL
        - SKU Name: Standard_LRS
    - **Storage Account 2:**
        - dataConfidentialityLevel: SENSITIVE
        - SKU Name: Standard_ZRS
    - **Storage Account 3:**
        - dataConfidentialityLevel: PROTECTED
        - SKU Name: Standard_GRS  

### Tips

- Documentation Link [User-defined data types in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/user-defined-data-types)
- [Parameters in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/parameters)
- [Set name and type for child resources in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/child-resource-name-type)
- [Resource dependencies in Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/resource-dependencies)


### Possible Solutions

```powershell
$VNetResourceGroup = (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-vnet-*").ResourceGroupName
(Get-Content "C:\Bicep\main.bicep").Replace('%VNET_RESOURCE_GROUP%', "$($VNetResourceGroup)") | Set-Content "C:\Bicep\main.bicep"

# Remove existing storage accounts before continuing
Get-AzStorageAccount | Remove-AzStorageAccount -Force

$StorageAccount1 = @{dataConfidentialityLevel = "OFFICIAL"; skuName = "Standard_LRS"}
$StorageAccount2 = @{dataConfidentialityLevel = "SENSITIVE"; skuName = "Standard_ZRS"}
$StorageAccount3 = @{dataConfidentialityLevel = "PROTECTED"; skuName = "Standard_GRS"}
$StorageAccounts = $StorageAccount1, $StorageAccount2, $StorageAccount3
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -Force -storageAccounts $StorageAccounts
```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/11)


## Challenge 12 - Linting and Testing

### Challenges

1. In this challenge you will validate your Bicep files with linting and testing. Linting and testing your Infrastructure as Code (IaC) as early as possible in the development cycle improves quality.
1. In C:\Bicep, create a <code>bicepconfig.json</code> file, and configure the linter so that the level is <code>error</code> for both <code>adminusername-should-not-be-literal</code> and <code>no-unused-params</code>.
1. Important: Remove all comments e.g. // from your bicepconfig.json file.
1. Update your Bicep file to include a default value for the parameter that declares your Storage Account properties, or add a parameter file. 
1. Install the following PowerShell Modules (For all users):- 
    - PSRule
    - PSRule.Rules.Azure
1. Install the following Visual Studio Code Extensions for PSRule:
    - PowerShell
    - PSRule

1. In C:\Bicep create a <code>ps-rule.yaml</code> file with the contents from the below link:
    - PSRule Configuration - https://raw.githubusercontent.com/waynehoggett/HackathonSetup/refs/heads/main/bicep/solutions/12/ps-rule.yaml
1.  Test your Bicep file using PSRule using the Visual Studio Code task PSRule: Run Analysis or the PowerShell cmdlet Assert-PSRule. 


### Tips

- [Add linter settings in the Bicep config file](https://learn.microsoft.com/azure/azure-resource-manager/bicep/bicep-config-linter)
- [Visual Studio Marketplace - PSRule V2](https://marketplace.visualstudio.com/items?itemName=bewhite.psrule-vscode)

### Possible Solutions

```powershell
$VNetResourceGroup = (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-vnet-*").ResourceGroupName
(Get-Content "C:\Bicep\main.bicep").Replace('%VNET_RESOURCE_GROUP%', "$($VNetResourceGroup)") | Set-Content "C:\Bicep\main.bicep"

Install-Module -Name PSRule -Scope AllUsers -Force
Install-Module -Name 'PSRule.Rules.Azure' -Scope AllUsers -Force
code --install-extension ms-vscode.PowerShell
code --install-extension bewhite.psrule-vscode
```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/12)


## Challenge 13 - Convert Existing Resources

### Challenges

1. In this challenge you will convert existing resources to infrastructure-as-code using Bicep.
1. Create a file named <code>vnet.bicep</code>. Add the required configuration to deploy the existing virtual network named <code>$LabVnetName</code> in the resource group <code>$VNetLabResourceGroupName</code> to your bicep file, retaining the existing configuration as-is.
1. Deploy the vnet.bicep Bicep file to <code>$VNetLabResourceGroupName</code>.

### Tips

- [Migrate Azure resources and JSON ARM templates to use Bicep](https://learn.microsoft.com/azure/azure-resource-manager/bicep/migrate)

### Possible Solutions

```powershell
$VNetResourceGroup = (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-vnet-*").ResourceGroupName
New-AzResourceGroupDeployment -ResourceGroupName $VNetResourceGroup -TemplateFile "C:\Bicep\vnet.bicep"

```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/13)


## Challenge 14 - Azure Verified Modules

### Challenges

1. In this challenge you will explore Azure Verified Modules (AVM). Azure Verified Modules provide standardized, Microsoft-supported Infrastructure-as-Code Possible Solutions that accelerate deployment, ensure best practices, and enhance reliability for Azure resources.
1. In main.bicep, use an Azure Verified Module to deploy <code>$LabVnetName</code> in the resource group <code>$VNetLabResourceGroupName</code>.
1. Delete <code>vnet.bicep</code>.
1. Deploy the main.bicep Bicep file.

### Tips

- [Azure Verified Modules (AVM) - Bicep Quickstart Guide](https://azure.github.io/Azure-Verified-Modules/usage/quickstart/bicep/#option-1-use-the-bicep-extension-in-vs-code)

### Possible Solutions

```powershell
$VNetResourceGroup = (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-vnet-*").ResourceGroupName
(Get-Content "C:\Bicep\main.bicep").Replace('%VNET_RESOURCE_GROUP%', "$($VNetResourceGroup)") | Set-Content "C:\Bicep\main.bicep"
$StorageAccount1 = @{dataConfidentialityLevel = "OFFICIAL"; skuName = "Standard_LRS"}
$StorageAccount2 = @{dataConfidentialityLevel = "SENSITIVE"; skuName = "Standard_ZRS"}
$StorageAccount3 = @{dataConfidentialityLevel = "PROTECTED"; skuName = "Standard_GRS"}
$StorageAccounts = $StorageAccount1, $StorageAccount2, $StorageAccount3
New-AzResourceGroupDeployment -ResourceGroupName (Get-AzResourceGroup | Where-Object ResourceGroupName -like "rg-lab-*").ResourceGroupName -TemplateFile "C:\Bicep\main.bicep" -Force -storageAccounts $StorageAccounts

```

- [See Additional Files](https://github.com/waynehoggett/HackathonSetup/tree/main/bicep/solutions/14)
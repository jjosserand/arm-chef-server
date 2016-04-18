# Creating a Chef Server using an ARM Template

This template will create a chef server on Azure.  The Chef server software will be installed but it will not be created.  This last part has to be done manually in this setup.

Ensure that the parameters file is up to date with the relevant settings before running the following commands.

## Run the template into Azure

There are two ways of running this in

### Option 1: Azure XPlat Command Line Tools

There are a few commands that are required to successfully deploy a template into Azure.

_Create the Resource group_

Unless an existing group is to be used to deploy the chef server to, an new one needs to be created.

```
$> azure group create "<NAME_OF_RESOURCE_GROUP>" -l "<AZURE_LOCATION>"
```

_Deploy the template to the resource group_

Once the group is available then the template can be deployed:

```
$> azure group deployment create -f chefserver.json -e chefserver.parameters.json <NAME_OF_RESOURCE_GROUP> <NAME_OF_DEPLOYMENT>
```

Once the task completes a new machine will exist in Azure.

### Option 2: PowerShell commands

If using Windows then the PowerShell commands for Azure can be used.

**Ensure that the Azure PowerShell package is installed - https://azure.microsoft.com/en-gb/documentation/articles/powershell-install-configure/**

_Create the Resource group_

Unless an existing group is to be used to deploy the chef server to, an new one needs to be created.

```
C:\> New-AzureRmResourceGroup -Name "<NAME_OF_RESOURCE_GROUP>" -Location "<AZURE_LOCATION>"
```

_Deploy the template to the resource group_

Once the group is available then the template can be deployed:

```
C:\> New-AzureRmResoureGroupDeployment -ResourceGroupName "<NAME_OF_RESOURCE_GROUP>" -TemplateFile chefserver.json -TemplateParameterFile chefserver.parameters.json -Name "<NAME_OF_DEPLOYMENT>"
```

## Configure the Chef Server

The Chef Server now needs to be configured.  This can be accomplished by logging onto the server and running the following commands.

```bash
$> echo 'api_fqdn "<FQDN>"' | sudo tee -a /etc/chef-marketplace/marketplace.rb
$> sudo chef-marketplace-ctl hostname <FQDN>
$> sudo chef-marketplace-ctl setup
```

More information can be found at https://docs.chef.io/azure_portal.html.

# Azure IoT Hub demonstration bundle using Duffle version 0.2.0-beta.4

Prerequisites:
1. [Duffle version 0.2.0-beta.4](https://github.com/deislabs/duffle/releases/tag/0.2.0-beta.4) for your Operating System. (If you try Windows, note that we haven't gotten a lot of Windows testers, so you're doing the due diligence we need.) 
2. An IoT Hub and IoT Device in Azure, in which at a minimum one device twin has tags.name == 'target'
3. An SP password for an SP that can be used to logon to Azure without 2FA and that has permissions to deploy to Azure IoT hubs. (This demo can be rerun by changing the password for the same SP.)
4. The SP values of tenant _by the domain name_ -- for example, Microsoft's is `microsoft.onmicrosoft.com` and the application ID.
5. An OCI runtime, like Docker. If you know how to use the `--driver` option in Duffle, then you might use other types of OCI runtimes like ACI. These are all known to be experimental.

## Build the bundle

To start, type `duffle init`, which will install the local infrastructure for Duffle. This means that if you've used a different version of Duffle before, you'll need to clean your ~/.duffle or %HOME%\.duffle directories first. 

From the root of this repository, type `duffle build .` Duffle will build the bundle's invocation image, and then the bundle. You can see the bundle's `bundle.json` file by typing `duffle bundle show cnab-iot-sql | jq` (the `jq` is to format the canonical json for readability, but you can use any tool).

## Create credentials to use the bundle

### Parameters

There are several parameters that the bundle can take. Look at the `duffle.json` file, which contains the following definitions. Note the defaults will only work on my demo environment:
    "CONDITION": 
    {
      "default": "target",
      "type": "string"
    },
    "APP_ID": 
    {
      "default": "http://demo-cli-login",
      "type": "string"
    },
    "DEPLOYMENT_ID": 
    {
      "default": "sql",
      "type": "string"
    },
    "AZURE_RESOURCE_GROUP": 
    {
      "default": "IoTEdgeResources",
      "type": "string"
    },
    "TENANT_DNS": 
    {
      "default": "microsoft.onmicrosoft.com",
      "type": "string"
    },
    "HUB_NAME": 
    {
      "default": "cnab-hub",
      "type": "string"
    }
  },

Of these, **`CONDITION`** is the value that the device twin needs to have for the `tags.name` metadata. It defaults to `target`, so you can use this bundle with any query that needs to target devices with `tags.name == "$CONDITION"` if you pass the `--set CONDITION=actual_value` option to duffle.

APP_ID is the automation SP App name: the default is mine, `https://demo-iot-cli`. Passing a new one is mandatory to use this bundle yourself.

DEPLOYMENT_ID is a string that will be the unique deployment name. 

AZURE_RESOURCE_GROUP is the rg that is associated with the Azure resources.

TENANT_DNS is the AAD domain for the SP. For Microsoft corporate accounts, this is `microsoft.onmicrosoft.com`, but can be changed for others to use.

HUB_NAME is the name of the controlling Azure IoT hub that is handling the target devices.

Assuming you have the service principal password you created to use inside the bundle as a way to log on to Azure, export that to the variable `SP_PASSWORD`. If you've already run the demo, you can reset the password and reuse the same values by typing 
> export SP_PASSWORD=$(az ad sp create-for-rbac --skip-assignment --name <my-iot-demo> --query '[password]' -o tsv)

where you replace `my-iot-demo` with the actual SP name. You will ALSO need the SP app name (not the ID), the tenant domain name for the SP. 

Once exported -- and it must be in POSIX worlds for `duffle` to have access to the environment variable -- type
> duffle creds generate iot-sql cnab-iot-sql  

You will be prompted to specify the name of the local environment variable the value of which to use as the AZURE_SP_PASSWORD as follows:
```
? Choose a source for "AZURE_SP_PASSWORD"  [Use arrows to move, space to select, type to filter]
  specific value
> environment variable
  file path
  shell command
```

Press `Enter` and then type `SP_PASSWORD`, or whatever environment variable you using to store the SP password.

You can see your credential by typing
> duffle creds list

and display its contents by
> duffle creds show iot-sql

the latter of which redacts the actual value:

```
name: iot-sql
credentials:
- name: PASSWORD
  source:
    value: REDACTED
```


## Create your parameters to use the bundle

## Install the bundle

## List the deployments using the Status command

## Purge the modules using a custom command

## Remove the deployments using a custom command





1. docker build --rm -f "cnab/Dockerfile" -t squillace/cnab-iot-sql:0.1.0 cnab && docker run -it --env-file deploy.64 squillace/cnab-iot-sql:0.1.0

2. az iot edge deployment list -g IoTEdgeResources -n cnab-hub --query '[].id' -o tsv | xargs -I {} az iot edge deployment delete -g IoTEdgeResources -d {} -n cnab-hub

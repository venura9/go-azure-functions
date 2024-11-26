# Azure Functions with Go

## Build and run 

```bash
go build server.go
./server
```

## Run the function locally

```bash
func init --custom
func new -l Custom -t HttpTrigger -n HttpExample -a anonymous
```

## Setting up the configration: 

1) Use Development Storage for local testing


`local.settings.json`
```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "custom",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true"
  }
}
```

2) Set the Custom Handler path & Turn on Forwarding

`host.json`
```
{
  "version": "2.0",
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "excludedTypes": "Request"
      }
    }
  },
  "extensionBundle": {
    "id": "Microsoft.Azure.Functions.ExtensionBundle",
    "version": "[4.*, 5.0.0)"
  },
  "customHandler": {
    "description": {
      "defaultExecutablePath": "server",
      "workingDirectory": "",
      "arguments": []
    },
    "enableForwardingHttpRequest": true
  }
}
```

## Deploy to Azure: 

```bash
FUNCTION_APP_NAME=azurebuilders

GOOS=linux GOARCH=amd64 go build server.go
func azure functionapp publish $FUNCTION_APP_NAME

```

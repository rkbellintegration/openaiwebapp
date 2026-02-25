# Azure OpenAI Web App

This repository contains a Blazor interactive server application demonstrating integration with Azure OpenAI's chat service (`gpt-4o-mini`).

## Requirements

- .NET 10.0 SDK
- Azure CLI logged in to a subscription with an Azure OpenAI resource
- `AZURE_OPENAI_ENDPOINT` and `AZURE_OPENAI_KEY` set as environment variables or in appsettings files

## Build & Run Locally

```bash
# restore and build
dotnet build

# run in the workspace (will use appsettings.Development.json)
dotnet run

# to run with env variables explicitly set:
export AZURE_OPENAI_ENDPOINT=https://<your-endpoint>.openai.azure.com/
export AZURE_OPENAI_KEY=<your-api-key>
dotnet run
```

## Publish and Deploy to Azure App Service

1. **Create resource group, plan, and web app** (example names shown):
   ```bash
   az group create --name rg-codespace --location eastus2
   az appservice plan create --name codespacesplan --resource-group rg-codespace --sku B1 --is-linux
   az webapp create --name codespacesblankapp --resource-group rg-codespace --plan codespacesplan --runtime "DOTNETCORE|10.0"
   ```

2. **Configure OpenAI settings on the web app**:
   ```bash
   az webapp config appsettings set \
     --resource-group rg-codespace \
     --name codespacesblankapp \
     --settings AZURE_OPENAI_ENDPOINT="https://<your-endpoint>.openai.azure.com/" AZURE_OPENAI_KEY="<your-api-key>"
   ```

3. **Publish and zip the project**:
   ```bash
   dotnet publish -c Release -o publish
   cd publish
   zip -r ../app.zip .
   cd ..
   ```

4. **Deploy**:
   ```bash
   az webapp deploy --resource-group rg-codespace --name codespacesblankapp --src-path app.zip --type zip
   ```

Visit `https://codespacesblankapp.azurewebsites.net` (or your chosen name) to see the app.

## Notes

- `.gitignore` excludes build artifacts and sensitive settings.
- To update the key or endpoint, update the App Service application settings and redeploy.
- For continuous deployment, consider hooking the GitHub repo to Azure Pipelines or GitHub Actions.  

Happy coding! 

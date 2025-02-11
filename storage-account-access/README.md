# Azure Storage Account Network and Access Configuration

This Kusto Query Language (KQL) script retrieves detailed access configurations for Azure Storage Accounts, including network rules, private endpoints, and custom domain settings.

## Query Explanation

1. **Filter Resources**: The query starts by filtering resources to only include those of type `microsoft.storage/storageaccounts`.
2. **Extract Public Access**: Extends the results with `anonymousReadAccessPermitted` to show if public blob access is enabled.
3. **Network ACLs**: Extracts network access control list configurations from the properties.
4. **Network Rules**: Processes multiple network rule types:
   - Virtual Network Rules
   - IPv4 Address Rules
   - IPv6 Address Rules
   - Service Bypass Rules
5. **Empty Array Handling**: Uses `iif` and `array_length` to display 'None' for empty rule sets.
6. **Private Endpoints**: Shows configured private endpoint connections, displaying 'None' if none exist.
7. **Custom Domain**: Displays the custom domain name if configured, otherwise shows 'notConfigured'.

## Output
The output provides a comprehensive view of network access and domain settings for each storage account. Here's an example of what the output might look like:

| id | name | anonymousReadAccessPermitted | defaultNetworkAction | allowedVirtualNetworks | allowedIpv4Addresses | allowedIpv6Addresses | allowedBypassedServices | privateEndpointConnections | customDomain |
|----|------|----------------------------|---------------------|----------------------|-------------------|-------------------|----------------------|--------------------------|--------------|
| /subscriptions/... | mystorageaccount | true | Deny | None | ["192.168.1.0/24"] | None | ["AzureServices"] | None | storage.contoso.com |
| /subscriptions/... | storageprod01 | false | Allow | ["vnet1/subnet1"] | None | None | None | ["endpoint1"] | notConfigured |
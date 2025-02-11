# Find all FQDN's configured in your Application Gateway backends

This Kusto Query Language (KQL) script is used to retrieve the Fully Qualified Domain Names (FQDNs) of backend addresses from Azure Application Gateways.

## Query Explanation

1. **Filter Resources**: The query starts by filtering resources to only include those of type `microsoft.network/applicationgateways`.
2. **Expand Backend Address Pools**: It then expands the `properties.backendAddressPools` array to get individual backend address pools.
3. **Project FQDN**: The query projects the FQDN of the first backend address in each pool.
4. **Filter Non-Empty FQDNs**: Finally, it filters out any entries where the FQDN is empty.

## Output
The output of this query will be a list of FQDNs for the backend addresses of Azure Application Gateways. Here is an example of what the output might look like:

| fqdn                          |
|-------------------------------|
| backend1.example.com          |
| backend2.example.com          |
| backend3.example.com          |
| backend4.example.com          |
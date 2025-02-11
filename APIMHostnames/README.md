# Find API Management Custom Domain Certificates in Azure

This Kusto Query Language (KQL) script identifies custom domain configurations and their associated certificates within Azure API Management services, helping with certificate management and expiry tracking.

## Query Explanation

1. **Filter Resources**: The query starts by filtering resources to only include those of type `microsoft.apimanagement/service`.
2. **Expand Hostname Configurations**: It uses `mv-expand` to flatten the array of hostname configurations for each API Management instance.
3. **Extract Properties**: The query extends the results with specific hostname and certificate properties:
   - `hostName`: The custom domain name
   - `certificate`: Certificate object containing subject and expiry details
   - `certificateSubject`: Subject of the certificate
   - `certificateExpiry`: Expiration date of the certificate
   - `certificateKeyVaultId`: Reference to the Key Vault certificate if applicable
4. **Filter Custom Domains**: The query excludes default Azure domains by filtering out hostnames containing "azure-api.net".
5. **Project Fields**: Finally, it projects specific fields in this order:
   - Resource ID
   - Hostname
   - Key Vault Certificate ID
   - Certificate Subject
   - Certificate Expiry Date

## Output
The output of this query will be a list of custom domains and their certificate details. Here is an example of what the output might look like:

| id | hostName | certificateKeyVaultId | certificateSubject | certificateExpiry |
|----|----------|----------------------|-------------------|-------------------|
| /subscriptions/sub1/.../apim1 | api.contoso.com | /subscriptions/sub1/.../certificates/cert1 | CN=api.contoso.com | 2025-01-15T10:30:00Z |
| /subscriptions/sub1/.../apim2 | dev.fabrikam.com | /subscriptions/sub1/.../certificates/cert2 | CN=dev.fabrikam.com | 2024-12-01T08:00:00Z |
| /subscriptions/sub1/.../apim3 | api.woodgrove.com | null | CN=api.woodgrove.com | 2024-11-15T09:20:00Z |
| /subscriptions/sub1/.../apim4 | portal.contoso.com | /subscriptions/sub1/.../certificates/cert4 | CN=portal.contoso.com | 2025-02-10T12:00:00Z |
# Find Virtual Machines without Automatic Patch Assessment in Azure

This Kusto Query Language (KQL) script identifies Windows Virtual Machines that are not configured for automatic patch assessment by the Azure platform, helping with patch management and compliance monitoring.

## Query Explanation

1. **Filter Resources**: The query starts by filtering resources to only include those of type `microsoft.compute/virtualmachines`.
2. **Extract Assessment Configuration**: It extends the results with the patch assessment mode:
   - `periodicAssessment`: The configured assessment mode from the Windows configuration patch settings
3. **Filter Non-Compliant VMs**: The query filters for VMs that don't have automatic assessment enabled:
   - Specifically, it finds VMs where `assessmentMode` is not set to "AutomaticByPlatform"
4. **Project Fields**: Finally, it projects specific fields in this order:
   - VM Name
   - Assessment Mode Configuration
   - Full Properties Object

## Output
The output of this query will be a list of Windows VMs that don't have automatic patch assessment enabled. Here is an example of what the output might look like:

| name | periodicAssessment | properties |
|------|-------------------|-------------|
| vm-prod-01 | ImageDefault | {<full properties object>} |
| vm-dev-02 | Manual | {<full properties object>} |
| vm-test-03 | ImageDefault | {<full properties object>} |
| vm-prod-04 | null | {<full properties object>} |

## Usage Notes
This query is particularly useful for:
- Identifying Windows VMs that need patch assessment configuration
- Finding machines that might be missing security updates
- Ensuring compliance with organizational patch management policies
- Preparing for enabling automatic patching across your environment
- Determining which VMs are using default image settings instead of explicit configuration
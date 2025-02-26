# Auditing Patch Installations in Azure

This Kusto Query Language (KQL) script identifies failed patch installations across Azure Virtual Machines, helping with troubleshooting and remediation of patch management issues.

## Query Explanation

1. **Filter Resources**: The query starts by filtering `patchinstallationresources` to only include software patches from patch installation results.
2. **Filter Failed Patches**: It extends the results with installation state and filters for patches with a "Failed" state.
3. **Extract Resource Details**: The query extracts several important properties:
   - `patchId`: The ID of the patch installation result
   - `virtualMachineName`: The name of the virtual machine where the patch failed
   - `classification`: The category of the patch (Security, Critical, etc.)
   - `rebootRequired`: Whether a reboot is required for this patch
   - `rebootBehaviour`: The configured reboot behavior
   - `patchName`: The name of the patch
   - `severity`: The MSRC severity rating, defaulting to 'N/A' if not available
   - `dateTime`: The timestamp when the patch was last modified
   - `kb`: The Microsoft Knowledge Base article ID for the patch
4. **Sort Results**: The query sorts the results by date/time in descending order (newest first).
5. **Project Fields**: Finally, it projects the fields in a specific order for clearer reporting.

## Output
The output of this query will be a list of failed patch installations with detailed information. Here is an example of what the output might look like:

| dateTime | patchId | virtualMachineName | kb | classification | severity | patchName | installationState | rebootRequired | rebootBehaviour |
|----------|---------|-------------------|-----|----------------|----------|-----------|-------------------|----------------|-----------------|
| 2024-02-20T15:30:00Z | patch-result-001 | vm-prod-01 | KB5022282 | Security | Critical | 2023-01 Security Update | Failed | True | Never |
| 2024-02-19T12:45:00Z | patch-result-002 | vm-dev-02 | KB5022894 | Security | Important | 2023-02 Cumulative Update | Failed | True | IfRequired |
| 2024-02-15T09:20:00Z | patch-result-003 | vm-test-03 | KB5023697 | Critical | Critical | 2023-03 Cumulative Update | Failed | True | Always |
| 2024-02-10T18:15:00Z | patch-result-004 | vm-prod-04 | KB5022906 | Security | Moderate | 2023-02 Security Update | Failed | False | Never |

## Usage Notes
This query is particularly useful for:
- Identifying failed patch installations that need remediation
- Prioritizing patch fixes based on severity and classification
- Tracking patch failures across your environment
- Identifying patterns of recurring patch failures
- Correlating patch failures with specific Windows updates
- Planning reboot windows for patches that require them
- Creating reports for security compliance
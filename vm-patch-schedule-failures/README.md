# Find Failed Patch Schedule Installation Results

This Kusto Query Language (KQL) script identifies Azure Virtual Machines with failed patch installation results and extracts the specific error details, helping with troubleshooting and remediation of patch management issues.

## Query Explanation

1. **Filter Resources**: The query starts by filtering `patchinstallationresources` to only include patch installation results for virtual machines.
2. **Extract VM Name**: It extracts the virtual machine name from the resource ID.
3. **Expand Error Details**: The query uses `mv-expand` to flatten the array of error details.
4. **Clean Error Messages**: It extends the results with cleaned error messages:
   - `errorDetail`: Removes quotes, brackets, and periods from the error message for cleaner output
5. **Filter Non-Successful Results**: The query filters for patch operations that did not succeed.
6. **Extract Timestamp**: It captures the last modified date/time for the patch operation.
7. **Remove Duplicates**: The query uses `distinct` to show only unique combinations of VM names and error details.
8. **Project Fields**: Finally, it projects specific fields in this order:
   - Date/Time of the patch operation
   - Virtual Machine Name
   - Error Detail
   - Update Status

## Output
The output of this query will be a list of unique VMs and their patch failure error messages. Here is an example of what the output might look like:

| dateTime | virtualMachineName | errorDetail | updateStatus |
|----------|-------------------|------------|--------------|
| 2024-02-20T15:30:00Z | vm-prod-01 | The VM is turned off and cannot be patched | Failed |
| 2024-02-19T12:45:00Z | vm-dev-02 | Not enough disk space to apply updates | Failed |
| 2024-02-15T09:20:00Z | vm-test-03 | Timeout occurred while waiting for update installation | Failed |
| 2024-02-10T18:15:00Z | vm-prod-04 | Access denied to Windows Update service | Failed |

## Usage Notes
This query is particularly useful for:
- Identifying the root causes of patch failures
- Grouping VMs by common error patterns
- Troubleshooting patch management issues
- Creating remediation plans based on specific error types
- Monitoring the health of your patch management solution
- Reporting on patch installation status across your environment
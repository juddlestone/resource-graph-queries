# Find Virtual Machine Patch Assessment Status in Azure

This Kusto Query Language (KQL) script identifies available patches for Azure Virtual Machines, helping with patch management and compliance monitoring.

## Query Explanation

1. **Filter Resources**: The query starts by filtering `patchassessmentresources` to only include those of type `microsoft.compute/virtualmachines/patchassessmentresults`.
2. **Extract VM Name**: It extracts the virtual machine name from the resource ID:
   - Uses `split()` to break the ID into segments
   - Takes the third-to-last segment using index `-3`
3. **Extract Properties**: The query extends the results with specific properties:
   - `operatingSystem`: The OS type of the virtual machine
   - `availablePatches`: An object containing counts of available patches by classification
4. **Sort Results**: The query sorts the results by operating system in descending order.
5. **Project Fields**: Finally, it projects specific fields in this order:
   - Virtual Machine Name
   - Operating System Type
   - Available Patches by Classification

## Output
The output of this query will be a list of virtual machines and their patch status. Here is an example of what the output might look like:

| virtualMachine | operatingSystem | availablePatches |
|----------------|-----------------|------------------|
| vm-prod-01 | Windows | {"Critical": 2, "Security": 5, "Other": 10} |
| vm-dev-02 | Windows | {"Critical": 0, "Security": 3, "Other": 7} |
| vm-test-03 | Linux | {"Security": 12, "Other": 15} |
| vm-prod-04 | Linux | {"Security": 8, "Other": 5} |

## Usage Notes
This query is particularly useful for:
- Identifying machines requiring critical security updates
- Planning patch maintenance windows
- Monitoring patch compliance across your environment
- Differentiating patch status between Windows and Linux machines
- Generating patch status reports for compliance requirements
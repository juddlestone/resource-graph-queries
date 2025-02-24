# Find Resource Maintenance Configuration Assignments in Azure

This Kusto Query Language (KQL) script identifies Azure resources that have maintenance configurations assigned to them, helping with maintenance window tracking and resource management.

## Query Explanation

1. **Filter Resources**: The query starts by filtering `maintenanceresources` to only include those of type `microsoft.maintenance/configurationassignments`.
2. **Extract Resource Details**: It extends the results with information about the associated resource:
   - `resourceName`: The name of the resource that has a maintenance configuration assigned
   - `resourceType`: The type of the resource (VM, Storage Account, etc.)
3. **Extract Maintenance Configuration**: The query extracts maintenance configuration details:
   - `maintenanceConfigurationId`: The full resource ID of the maintenance configuration
   - `maintenanceConfigurationName`: The name of the maintenance configuration extracted from the ID
4. **Project Fields**: Finally, it projects specific fields in this order:
   - Maintenance Configuration Name
   - Resource Name

## Output
The output of this query will be a list of resources and their assigned maintenance configurations. Here is an example of what the output might look like:

| maintenanceConfigurationName | resourceName |
|------------------------------|--------------|
| config-weekly-sunday | vm-prod-01 |
| config-monthly-patch | vm-dev-02 |

## Usage Notes
This query is particularly useful for:
- Identifying which resources have maintenance configurations
- Planning for upcoming maintenance windows
- Auditing maintenance configuration coverage across environments
- Ensuring critical resources have appropriate maintenance schedules
- Creating reports of maintenance assignments for operational teams
# Find Orphaned and Unattached Disks in Azure

This Kusto Query Language (KQL) script is used to identify orphaned and unattached managed disks in your Azure environment, helping with resource optimization and cost management.

## Query Explanation

1. **Filter Resources**: The query starts by filtering resources to only include those of type `microsoft.compute/disks`.
2. **Extract Properties**: It extends the results with specific disk properties:
   - `diskState`: Current state of the disk
   - `diskTier`: Storage tier of the disk
   - `timeCreated`: When the disk was created
   - `diskSizeGB`: Size of the disk in gigabytes
   - `sku`: Storage SKU name
3. **Handle Ownership Time**: The query processes the `lastOwnershipUpdateTime`, setting it to "neverAttached" if the value is null or empty, otherwise using the actual timestamp.
4. **Filter Unattached Disks**: The query filters for disks that are either unmanaged (`managedBy == ""`) or in an unattached state.
5. **Exclude ASR Replicas**: It excludes Azure Site Recovery replica disks by filtering out names containing "-ASRReplica".
6. **Project Fields**: Finally, it projects specific fields in this order:
   - Disk ID
   - Name
   - Resource Group
   - SKU
   - Disk Size GB
   - Disk Tier
   - Last Ownership Update Time
   - Time Created
   - Location
   - Tags

## Output
The output of this query will be a list of unattached disks with their properties. Here is an example of what the output might look like:

| id | name | resourceGroup | sku | diskSizeGB | diskTier | lastOwnershipUpdateTime | timeCreated | location | tags |
|----|------|--------------|-----|------------|-----------|------------------------|-------------|-----------|------|
| /subscriptions/sub1/.../disk1 | disk1 | rg-prod | Premium_LRS | 256 | P30 | neverAttached | 2024-01-15T10:30:00Z | eastus | {"env": "prod"} |
| /subscriptions/sub1/.../disk2 | disk2 | rg-dev | Standard_LRS | 128 | Standard | 2024-02-01T15:45:00Z | 2023-12-01T08:00:00Z | westus | {"env": "dev"} |
| /subscriptions/sub1/.../disk3 | disk3 | rg-test | Premium_LRS | 512 | P40 | 2024-01-20T08:15:00Z | 2023-11-15T09:20:00Z | centralus | {"env": "test"} |
| /subscriptions/sub1/.../disk4 | disk4 | rg-prod | Standard_LRS | 64 | Standard | neverAttached | 2024-02-10T12:00:00Z | eastus2 | {"env": "prod"} |
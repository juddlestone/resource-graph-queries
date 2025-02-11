# Find Network Interface and Subnet NSG Conflicts in Azure

This Kusto Query Language (KQL) script identifies potential security conflicts where both a Network Interface (NIC) and its associated subnet have Network Security Groups (NSGs) attached, which could lead to complex or unexpected network security behaviour.

## Query Explanation

1. **Filter Network Interfaces**: The query starts by filtering resources to only include Network Interfaces (`microsoft.network/networkinterfaces`).
2. **Filter NICs with NSGs**: It narrows down to NICs that have an NSG attached by checking if the `networkSecurityGroup` property is not empty.
3. **Extract NIC Properties**: The query extends the results with specific NIC properties:
   - `nicNsgId`: The resource ID of the NSG attached to the NIC
   - `subnetId`: The resource ID of the subnet the NIC is connected to
4. **Join with Virtual Networks**: It performs a left outer join with Virtual Network resources to get subnet NSG information:
   - Expands the subnets array for each VNet
   - Extracts the subnet ID and its associated NSG ID
5. **Filter Valid Conflicts**: The query filters for cases where:
   - The subnet has an NSG (`isnotempty(subnetNsgId)`)
   - The NIC is associated with that subnet
6. **Project Fields**: Finally, it projects specific fields in this order:
   - Subscription ID
   - Resource Group
   - NIC Name
   - NIC NSG ID
   - Subnet ID
   - Subnet NSG ID

## Output
The output of this query will be a list of NICs where both the NIC and its subnet have NSGs attached. Here is an example of what the output might look like:

| subscriptionId | resourceGroup | name | nicNsgId | subnetId | subnetNsgId |
|----------------|---------------|------|----------|-----------|-------------|
| sub1 | rg-prod | nic-prod-01 | /subscriptions/sub1/.../nsg1 | /subscriptions/sub1/.../subnet1 | /subscriptions/sub1/.../nsg2 |
| sub1 | rg-dev | nic-dev-02 | /subscriptions/sub1/.../nsg3 | /subscriptions/sub1/.../subnet2 | /subscriptions/sub1/.../nsg4 |
| sub2 | rg-test | nic-test-03 | /subscriptions/sub2/.../nsg5 | /subscriptions/sub2/.../subnet3 | /subscriptions/sub2/.../nsg6 |
| sub2 | rg-prod | nic-prod-04 | /subscriptions/sub2/.../nsg7 | /subscriptions/sub2/.../subnet4 | /subscriptions/sub2/.../nsg8 |

## Usage Notes
This query is particularly useful for:
- Identifying potential network security complexity
- Auditing network security configurations
- Finding instances where traffic might be subject to multiple NSG rule evaluations
- Simplifying network security management by identifying where NSG consolidation might be beneficial
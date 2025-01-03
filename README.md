# Configuring Private Link in Azure Databricks

## 1. Prerequisites

Before you begin, ensure you have:

- Permissions to create and manage resources in Azure.
- A Virtual Network (VNet) configured for Azure Databricks.
- A dedicated subnet for private endpoints.
- Databricks configured with VNet Injection.

## 2. Configure Private Link in Azure Databricks

### Enable Private Link in the Databricks Workspace:

1. In the Azure portal, go to the Azure Databricks resource.
2. Click **Networking**.
3. Enable the Private Link configuration.
4. Ensure the control plane and data plane subnets are configured to support Private Link.

### Create a Private Endpoint:

1. In the Azure portal, navigate to **Private Link Center** and select **Private Endpoints**.
2. Click **+ Create**.
3. Configure the following:
   - **Resource Type**: Azure Databricks.
   - **Target Sub-resource**: Workspace.
   - **Virtual Network**: Select the VNet associated with Databricks.
   - **Subnet**: Choose a dedicated subnet for private endpoints.
4. Click **Review + Create**.

### Associate the VNet with the Private Endpoint:

1. Ensure the VNet used by Databricks has access to the private endpoint.
2. Configure routing rules to ensure traffic destined for Databricks is routed through the private network.

## 3. Configure DNS to Resolve Private Link

### Update Local DNS:

- If using a custom DNS, configure entries to direct Databricks traffic to the private endpoint.
- Use Azure DNS to automatically manage DNS records.

### Validate DNS Resolution:

- Use tools like `nslookup` or `dig` to confirm that Databricks addresses resolve to the private endpoint.

## 4. Update Firewall Rules

1. In the Azure portal, go to the firewall resource.
2. Add the necessary rules to allow traffic from your VNet to Databricks:
   - Select the private endpoint subnet as the source.
   - Configure the required ports (e.g., 443 for HTTPS).

## 5. Test the Connection

1. Create a cluster in Databricks and ensure it uses the Private Link.
2. Perform a connectivity test to confirm:
   - The cluster is not using public IPs.
   - Traffic flows through the private network.

## 6. Optional: Configure Data Plane for Private Communication

If needed, configure Data Plane resources (e.g., storage, SQL) to also use private endpoints:

1. Set up Private Endpoints for resources like Azure Storage, SQL, etc.
2. Update Databricks to use these private endpoints for communication.

# Referenced videos and documentation

https://www.youtube.com/watch?v=VbK0KYgpE0U

https://www.youtube.com/watch?v=xLzIO7N3wV4

https://learn.microsoft.com/en-us/azure/databricks/security/network/front-end/front-end-private-connect

https://learn.microsoft.com/en-us/azure/databricks/security/network/classic/private-link

# prefect-azure

`prefect-azure` makes it easy to leverage the capabilities of Azure in your workflows.
For example, you can retrieve secrets, read and write Blob Storage objects, and deploy your flows on Azure Container Instances (ACI).

## Getting Started

### Prerequisites

- [Prefect installed](https://docs.prefect.io/latest/getting-started/installation/) in a virtual environment.
- An [Azure account](https://azure.microsoft.com/) and the necessary permissions to access desired services.

### Install prefect-azure

<div class = "terminal">
```bash
pip install -U prefect-azure
```
</div>

If necessary, see [additional installation options for Blob Storage, Cosmos DB, and ML Datastore](#additional-installation-options).

To install with all additional functionality, use the following command:

<div class = "terminal">
```bash
pip install -U "prefect-azure[all_extras]"
```
</div>

### Register newly installed block types

Register the block types in the module to make them available for use.

<div class = "terminal">
```bash
prefect block register -m prefect_azure
```
</div>

## Examples

### Download a blob

```python
from prefect import flow

from prefect_azure import AzureBlobStorageCredentials
from prefect_azure.blob_storage import blob_storage_download

@flow
def example_blob_storage_download_flow():
    connection_string = "connection_string"
    blob_storage_credentials = AzureBlobStorageCredentials(
        connection_string=connection_string,
    )
    data = blob_storage_download(
        blob="prefect.txt",
        container="prefect",
        azure_credentials=blob_storage_credentials,
    )
    return data

example_blob_storage_download_flow()
```

Use `with_options` to customize options on any existing task or flow:

```python
custom_blob_storage_download_flow = example_blob_storage_download_flow.with_options(
    name="My custom task name",
    retries=2,
    retry_delay_seconds=10,
)
```

### Run flows on Azure Container Instances

Run flows on [Azure Container Instances (ACI)](https://learn.microsoft.com/en-us/azure/container-instances/) to dynamically scale your infrastructure.

See the [Azure Container Instances Worker Guide](/2.19.4/aci_worker/) for a walkthrough of using ACI in a hybrid work pool.

If you're using Prefect Cloud, [ACI push work pools](/2.19.4/guides/deployment/push-work-pools/#__tabbed_1_2) provide all the benefits of ACI with a quick setup and no worker needed.

## Resources

For assistance using Azure, consult the [Azure documentation](https://learn.microsoft.com/en-us/azure).

Refer to the prefect-azure API documentation linked in the sidebar to explore all the capabilities of the prefect-azure library.

### Additional installation options

To use Blob Storage:

<div class="terminal">
```bash
pip install -U "prefect-azure[blob_storage]"
```
</div>

To use Cosmos DB:

<div class="terminal">
```bash
pip install -U "prefect-azure[cosmos_db]"
```
</div>

To use ML Datastore:

<div class="terminal">
```bash
pip install -U "prefect-azure[ml_datastore]"
```
</div>

# Setup instructions for the Review metadata add-in for Microsoft Excel

To give users in an organization the option to work on data class and term assignments in Microsoft Excel instead of the metadata enrichment UI in Cloud Pak for Data as a Service, a Microsoft admin in the organization must complete several tasks:

- Download the resources from this repository.
- Deploy and publish the add-in.
- Make the workbook template available to users.

## Downloading resources

Download the `manifest.xml` file and the `Review metadata - IBM Knowledge Catalog.xlsx` Excel workbook template.

**Note:** The URL in the `SupportURL` element in the `manifest.xml` file points to the IBM Knowledge Catalog documentation. If you want to link to any custom documentation instead, replace the URL as required.

## Publishing the add-in

To deploy and publish the Review metadata add-in, follow the instructions in the Microsoft documentation: [Deploy and publish Office Add-ins](https://learn.microsoft.com/en-us/office/dev/add-ins/publish/publish)


## Making the workbook template available

Ensure that users in organization have access to an up-to-date version of the Excel workbook template for reviewing metadata in context of IBM Knowledge Catalog.

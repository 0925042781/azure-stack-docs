---
title: Title | Microsoft Docs
description: description
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila

ms.assetid:
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: mabrigg

---

# Supported metrics with Azure Monitor on Azure Stack
To be consistent with Azure, Azure monitor on Azure Stack provides same ways to interact with metrics, including charting them in the portal, accessing them through the REST API, or querying them using PowerShell or CLI. 
Below is a list of the all metrics currently available with Azure Monitor's metric pipeline on Azure Stack, it only includes metrics available using the consolidated Azure Monitor metric pipeline. To query for and access these metrics please use the 2018-01-01 api-version. 

##Microsoft.Compute/virtualMachines
| Metric | Metric Display Name | Unit | Aggregation Type | Description | Dimensions |
|----------------|---------------------|---------|------------------|-----------------------------------------------------------------------------------------------|---------------|
| Percentage CPU | Percentage CPU | Percent | Average | The percentage of allocated compute units that are currently in use by the Virtual Machine(s) | No Dimensions |

## Microsoft.Storage/storageAccounts
| Metric | Metric Display Name | Unit | Aggregation Type | Description | Dimensions |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| UsedCapacity | Used capacity | Bytes | Average | Account used capacity | No Dimensions |
| Transactions | Transactions | Count | Total | The number of requests made to a storage service or the specified API operation. This number includes successful and failed requests, as well as requests which produced errors. Use ResponseType dimension for the number of different type of response. | ResponseType, GeoType, ApiName |
| Ingress | Ingress | Bytes | Total | The amount of ingress data, in bytes. This number includes ingress from an external client into Azure Storage as well as ingress within Azure. | GeoType, ApiName |
| Egress | Egress | Bytes | Total | The amount of egress data, in bytes. This number includes egress from an external client into Azure Storage as well as egress within Azure. As a result, this number does not reflect billable egress. | GeoType, ApiName |
| SuccessServerLatency | Success Server Latency | Milliseconds | Average | The average latency used by Azure Storage to process a successful request, in milliseconds. This value does not include the network latency specified in AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Success E2E Latency | Milliseconds | Average | The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. This value includes the required processing time within Azure Storage to read the request, send the response, and receive acknowledgment of the response. | GeoType, ApiName |
| Availability | Availability | Percent | Average | The percentage of availability for the storage service or the specified API operation. Availability is calculated by taking the TotalBillableRequests value and dividing it by the number of applicable requests, including those that produced unexpected errors. All unexpected errors result in reduced availability for the storage service or the specified API operation. | GeoType, ApiName |

## Microsoft.Storage/storageAccounts/blobServices
| Metric | Metric Display Name | Unit | Aggregation Type | Description | Dimensions |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| BlobCapacity | Blob Capacity | Bytes | Total | The amount of storage used by the storage account’s Blob service in bytes. | BlobType |
| BlobCount | Blob Count | Count | Total | The number of Blob in the storage account’s Blob service. | BlobType |
| ContainerCount | Blob Container Count | Count | Average | The number of containers in the storage account’s Blob service. | No Dimensions |
| Transactions | Transactions | Count | Total | The number of requests made to a storage service or the specified API operation. This number includes successful and failed requests, as well as requests which produced errors. Use ResponseType dimension for the number of different type of response. | ResponseType, GeoType, ApiName |
| Ingress | Ingress | Bytes | Total | The amount of ingress data, in bytes. This number includes ingress from an external client into Azure Storage as well as ingress within Azure. | GeoType, ApiName |
| Egress | Egress | Bytes | Total | The amount of egress data, in bytes. This number includes egress from an external client into Azure Storage as well as egress within Azure. As a result, this number does not reflect billable egress. | GeoType, ApiName |
| SuccessServerLatency | Success Server Latency | Milliseconds | Average | The average latency used by Azure Storage to process a successful request, in milliseconds. This value does not include the network latency specified in AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Success E2E Latency | Milliseconds | Average | The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. This value includes the required processing time within Azure Storage to read the request, send the response, and receive acknowledgment of the response. | GeoType, ApiName |
| Availability | Availability | Percent | Average | The percentage of availability for the storage service or the specified API operation. Availability is calculated by taking the TotalBillableRequests value and dividing it by the number of applicable requests, including those that produced unexpected errors. All unexpected errors result in reduced availability for the storage service or the specified API operation. | GeoType, ApiName |

## Microsoft.Storage/storageAccounts/tableServices
| Metric | Metric Display Name | Unit | Aggregation Type | Description | Dimensions |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| TableCapacity | Table Capacity | Bytes | Average | The amount of storage used by the storage account’s Table service in bytes. | No Dimensions |
| TableCount | Table Count | Count | Average | The number of table in the storage account’s Table service. | No Dimensions |
| TableEntityCount | Table Entity Count | Count | Average | The number of table entities in the storage account’s Table service. | No Dimensions |
| Transactions | Transactions | Count | Total | The number of requests made to a storage service or the specified API operation. This number includes successful and failed requests, as well as requests which produced errors. Use ResponseType dimension for the number of different type of response. | ResponseType, GeoType, ApiName |
| Ingress | Ingress | Bytes | Total | The amount of ingress data, in bytes. This number includes ingress from an external client into Azure Storage as well as ingress within Azure. | GeoType, ApiName |
| Egress | Egress | Bytes | Total | The amount of egress data, in bytes. This number includes egress from an external client into Azure Storage as well as egress within Azure. As a result, this number does not reflect billable egress. | GeoType, ApiName |
| SuccessServerLatency | Success Server Latency | Milliseconds | Average | The average latency used by Azure Storage to process a successful request, in milliseconds. This value does not include the network latency specified in AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Success E2E Latency | Milliseconds | Average | The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. This value includes the required processing time within Azure Storage to read the request, send the response, and receive acknowledgment of the response. | GeoType, ApiName |
| Availability | Availability | Percent | Average | The percentage of availability for the storage service or the specified API operation. Availability is calculated by taking the TotalBillableRequests value and dividing it by the number of applicable requests, including those that produced unexpected errors. All unexpected errors result in reduced availability for the storage service or the specified API operation. | GeoType, ApiName |

## Microsoft.Storage/storageAccounts/queueServices
| Metric | Metric Display Name | Unit | Aggregation Type | Description | Dimensions |
|----------------------|------------------------|--------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| QueueCapacity | Queue Capacity | Bytes | Average | The amount of storage used by the storage account’s Queue service in bytes. | No Dimensions |
| QueueCount | Queue Count | Count | Average | The number of queue in the storage account’s Queue service. | No Dimensions |
| QueueMessageCount | Queue Message Count | Count | Average | The approximate number of queue messages in the storage account’s Queue service. | No Dimensions |
| Transactions | Transactions | Count | Total | The number of requests made to a storage service or the specified API operation. This number includes successful and failed requests, as well as requests which produced errors. Use ResponseType dimension for the number of different type of response. | ResponseType, GeoType, ApiName |
| Ingress | Ingress | Bytes | Total | The amount of ingress data, in bytes. This number includes ingress from an external client into Azure Storage as well as ingress within Azure. | GeoType, ApiName |
| Egress | Egress | Bytes | Total | The amount of egress data, in bytes. This number includes egress from an external client into Azure Storage as well as egress within Azure. As a result, this number does not reflect billable egress. | GeoType, ApiName |
| SuccessServerLatency | Success Server Latency | Milliseconds | Average | The average latency used by Azure Storage to process a successful request, in milliseconds. This value does not include the network latency specified in AverageE2ELatency. | GeoType, ApiName |
| SuccessE2ELatency | Success E2E Latency | Milliseconds | Average | The average end-to-end latency of successful requests made to a storage service or the specified API operation, in milliseconds. This value includes the required processing time within Azure Storage to read the request, send the response, and receive acknowledgment of the response. | GeoType, ApiName |
| Availability | Availability | Percent | Average | The percentage of availability for the storage service or the specified API operation. Availability is calculated by taking the TotalBillableRequests value and dividing it by the number of applicable requests, including those that produced unexpected errors. All unexpected errors result in reduced availability for the storage service or the specified API operation. | GeoType, ApiName |

## Next steps
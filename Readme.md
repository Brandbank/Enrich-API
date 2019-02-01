# Nielsen Brandbank - Enrich API

A JSON API for the creation and management of enrich assets.

# Version 1.0.0 Release Notes
* New endpoint for creating a single asset : 'PUT asset'
* New endpoint for creating assets in bulk : 'PUT bulk/asset'
* New endpoint for fetching request status : 'GET receipt/{receiptId}'
* Abillity to specificy a folder, new or existing, where an asset should be placed.

## Authentication
A 'user_credential' header must be supplied in all requests made to the API. 
This user credential should have been provided to you by Nielsen Brandbank when first signing up to use the Enrich API. 

# Swagger
All requests made to the API are validated against the following Swagger schema : ![swagger.yaml](swagger.yaml?raw=true "swagger api") 

Requests made to the API that do not pass this swagger schema will be rejected and return a 400 HTTP status code.

## Example Swagger Validation Error Response
```
{
    "message": "Failed to validate against schema TODO($ref to swagger schema) - Required properties are missing from object: correlationId. Path 'assets[0]', line 31, position 9."
}
```

# Endpoints 
## PUT api/bulk/asset
Creates a set of enrich assets. On success, returns a 202 HTTP status code along with a receipt ID which is used to query the status of the request.

A ``correlationId`` must be provided for each asset. This ``correlationId`` is later returned by ``GET receipt/{receiptId}`` to identify the created asset. The ``correlationId`` property must be unique within a request.

### Example Request
```
{
    "assets": [
        {
            "correlationId": "d81acecf-bd60-4a0e-a982-68d17c746c74",
            "asset": {
                "subcode": "OCOM058",
                "url": "https://www.gstatic.com/webp/gallery3/2.png",
                "embedded": false,
                "metadata": {
                    "name": "My Test Asset 1",
                    "ownerEmail": "test.user@brandbank.com",
                    "visibility": "public",
                    "assetStyle": "advert",
                    "assetType": "image",
                    "tags": [
                        "mytag",
                        "data"
                    ],
                    "folder": {
                        "name": "fruit",
                        "subfolder": {
                            "name": "apple",
                            "subfolder": {
                                "name": "adverts"
                            }
                        }
                    },
                    "liveDate": "2019-01-21",
                    "endDate": "2019-01-23"
                }
            }
        },
        {
            "correlationId": "c764132c-3ab3-4c0f-aad4-d741be1d7ca3",
            "asset": {
                "subcode": "OCOM058",
                "url": "https://www.gstatic.com/webp/gallery3/1.sm.png",
                "embedded": false,
                "metadata": {
                    "name": "My Test Asset 2",
                    "ownerEmail": "test.user@brandbank.com",
                    "visibility": "internal",
                    "tags": [
                        "mytag",
                        "data"
                    ],
                    "folder": {
                        "name": "fruit",
                        "subfolder": {
                            "name": "apple",
                            "subfolder": {
                                "name": "recipes"
                            }
                        }
                    },
                    "liveDate": "2019-01-21",
                    "endDate": "2019-01-23"
                }
            }
        },
        {
            "correlationId": "c764132c-3ab3-4c0f-aad4-d741be1d7ca3",
            "asset": {
                "subcode": "OCOM100",
                "url": "https://www.youtube.com/watch?v=m1vNNjEMMYs",
                "embedded": true,
                "metadata": {
                    "name": "My Test Asset 3",
                    "ownerEmail": "test.user@brandbank.com",
                    "visibility": "private",
                    "tags": [
                        "mytag",
                        "data"
                    ],
                    "folder": {
                        "name": "promo",
                        "subfolder": {
                            "name": "nielsen"
                        }
                    },
                    "liveDate": "2019-01-21",
                    "endDate": "2019-01-26"
                }
            }
        }
    ]
}
```
### Example Response
```
{
    "receiptId": "f7b3c859-d3dc-4831-b83b-9d2f3ffd9f98"
}
```
## PUT api/asset
Creates an enrich asset with the provided properties. On success, returns a 202 HTTP status code along with a receipt ID which is used to query the status of the request.

A ``correlationId`` must be provided along with the asset. This ``correlationId`` is later returned by ``GET receipt/{receiptId}`` to identify the created asset.

### Example Request
```
{
    "correlationId": "4dd5bd13-eff3-40e9-a30d-37f19253245c",
    "asset": {
        "subcode": "OCOM058",
        "url": "https://www.gstatic.com/webp/gallery3/2.png",
        "embedded": false,
        "metadata": {
            "name": "My Test Asset",
            "ownerEmail": "test.user@brandbank.com",
            "visibility": "public",
            "assetStyle": "advert",
            "assetType": "image",
            "tags": [
                "mytag",
                "data"
            ],
            "folder": {
                "name": "fruit",
                "subfolder": {
                    "name": "apple",
                    "subfolder": {
                        "name": "adverts"
                    }
                }
            },
            "liveDate": "2019-01-21",
            "endDate": "2019-01-23"
        }
    }
}
```
### Response
```
{
    "receiptId": "c04d1d94-bbb2-482c-82e6-e0b985d4315c"
}
```
## GET receipt/{receiptId}
Returns the status of a request.
### Example Response
```
{
    "receiptId": "940147af-2a95-45fe-80fb-118d0ccbeea8",
    "assets": [
        {
            "correlationId": "c764132c-3ab3-4c0f-aad4-d741be1d7ca3",
            "assetId": 108263,
            "assetVersionId": 110280,
            "status": "success",
            "type": "create",
            "messages": []
        },
        {
            "correlationId": "d81acecf-bd60-4a0e-a982-68d17c746c74",
            "assetId": null,
            "assetVersionId": null,
            "status": "pending",
            "type": "create",
            "messages": []
        },
        {
            "correlationId": "4fe194a3-45e7-4a44-b7c4-70a6a6a2e86e",
            "assetId": null,
            "assetVersionId": null,
            "status": "inProgress",
            "type": "create",
            "messages": []
        },
        {
            "correlationId": "e71d01fd-1d2d-4214-98f4-586f1db44c02",
            "assetId": null,
            "assetVersionId": null,
            "status": "failed",
            "type": "create",
            "messages": [
                {
                    "message": "The content at https://www.gstatic.com/webp/gallery3/1.sm.pnx has an invalid file extension .pnx",
                    "type": "invalidFileExtension",
                    "severity": "error"
                }
            ]
        },
        {
            "correlationId": "e266e3ca-8495-4d9a-abb1-58206e933561",
            "assetId": null,
            "assetVersionId": null,
            "status": "failed",
            "type": "create",
            "messages": [
                {
                    "message": "Invalid subcode OCOM057",
                    "type": "invalidSubcode",
                    "severity": "error"
                }
            ]
        }
    ]
}
```

## URL restrictions
- The content at the URL must be accessible without any need for authentication. For example a leased or SAS URL https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1
- The URL must be a document path (e.g. https://enrichcontent.storage.com/testing/image.png). If the URL is not a document path an attempt will be made to resolve the file extension from the ``content-type`` header.


## Versioning Stratergy
Versioning stratergy based on https://semver.org/

X.Y.Z | Major.Minor.Patch

Major : Breaking change

Minor : Non breaking change

Patch : Backwards compatible bug fixes

# Known Issues
- If the content at the URL takes longer than 10 minutes to download the asset operation will get stuck in an 'inProgress' state. 

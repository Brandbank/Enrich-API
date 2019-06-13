# Nielsen Brandbank - Enrich API

A JSON API for the creation and management of enrich assets.

# Version 2.0.0 (Product Linking) Release Notes  

* Support for adding product links via ``PUT bulk/asset`` and ``PUT asset`` using new ``linkedProducts`` array.
* New endpoints for managing product links on existing assets : ``POST link/{assetId}`` and ``POST bulk/link``
* Overhauled Swagger schema to be more specific, and to remove unused values

# Version 1.0.0 Release Notes
* New endpoint for creating a single asset : 'PUT asset'
* New endpoint for creating assets in bulk : 'PUT bulk/asset'
* New endpoint for fetching request status : 'GET receipt/{receiptId}'
* Ability to specify a folder, new or existing, where an asset should be placed.

## Authentication
A 'user_credential' header must be supplied in all requests made to the API.
This user credential should have been provided to you by Nielsen Brandbank when first signing up to use the Enrich API.

# Swagger
All requests made to the API are validated against the following Swagger schema : ![swagger.yaml](swagger.yaml?raw=true "swagger api")

Requests made to the API that do not pass this swagger schema will be rejected and return a 400 HTTP status code.

## Example Swagger Validation Error Response
```
{
   "message": "Failed to validate against schema https://raw.githubusercontent.com/Brandbank/Enrich-API/master/swagger.yaml - Required properties are missing from object: correlationId. Path 'assets[0]', line 31, position 9."
}
```

# Endpoints

Base URL : https://enrichapi.brandbank.com/enrich

## PUT bulk/asset
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
           "correlationId": "ecab3b94-e4a9-4844-9ed1-245484010847",
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
                   "linkedProducts": [
                       {
                           "subcode":"OCOM058",
                           "code":"5120190418012",
                           "codeType" : "EAN"
                       },
                       {
                           "subcode":"OCOM058",
                           "code":"5120190418028",
                           "codeType" : "EAN"
                       },
                       {
                           "subcode":"OCOM999",
                           "code":"5120190413122",
                           "codeType" : "EAN"
                       }
                   ],
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
                   "linkedProducts": [
                       {
                           "subcode":"OCOM058",
                           "code":"5120190418012",
                           "codeType" : "EAN"
                       },
                       {
                           "subcode":"OCOM058",
                           "code":"5120190418028",
                           "codeType" : "EAN"
                       },
                       {
                           "subcode":"OCOM999",
                           "code":"5120190413122",
                           "codeType" : "EAN"
                       }
                   ],
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
## PUT asset
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
           "linkedProducts": [
                {
                    "subcode":"OCOM058",
                    "code":"5120190418012",
                    "codeType" : "EAN"
                },
                {
                    "subcode":"OCOM058",
                    "code":"5120190418028",
                    "codeType" : "EAN"
                },
                {
                    "subcode":"OCOM999",
                    "code":"5120190413122",
                    "codeType" : "EAN"
                }
            ],
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

If there are any messages with severity ``error`` in the ``messages`` array then the asset operation status will be ``failed``.

If there are any messages with severity ``warning`` in the ``messages`` array then the asset operation status will be ``partialSuccess``.

If the asset is created successfully but linking products to the asset fails, the asset will have a status of ``partialSuccess``.

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
       },
       {
            "correlationId": "78339bed-26ca-44a4-936c-68f0224443c1",
            "assetId": 246736,
            "assetVersionId": 248773,
            "type": "create",
            "status": "partialSuccess",
            "messages": [
                {
                    "message": "Link Asset '246736' Product 'OCOM058/EAN/5120190418012' - The product 5120190418012 must be in Live, Pending Release or Awaiting Approval to be linked to an asset.",
                    "type": "invalidProductState",
                    "severity": "warning"
                }
            ]
        }
   ]
}
```

## POST bulk/link
Creates, and publishes, links between a given set of Enrich assets and the products to link to them. 

This process will completely replace any previously existing links against the asset, deleting any links not specified in the request body.

This endpoint will return a 200 status response, containing details on whether or not the requested product links were successfully applied.

An asset level operation can have a 'success', 'partialSuccess', or 'failed' status. A partial success is when some product links succeeded, but some failed.

Product level operations can have either 'success' or 'failed' status.

### Example Request
```
{
	"assets": [
		{
			"assetId": 129395,
			"linkedProducts": [
				{
					"subcode": "OCOM058",
					"codeType": "EAN",
					"code": "5051794017267"
				},
				{
					"subcode": "OCOM058",
					"codeType": "EAN",
					"code": "12341231234"
				}
			]
		},
		{
			"assetId": 129389,
			"linkedProducts": [
				{
					"subcode": "OCOM058",
					"codeType": "EAN",
					"code": "5051794017267"
				},
			]
		}
	]
}
```

### Example Response
```
{
  "requestId": "9ce6b393-3128-4d63-8a27-7239804e6ac5",
  "assets": [
    {
      "assetId": 129395,
      "linkedProducts": [
        {
          "product": {
            "subcode": "OCOM058",
            "codeType": "EAN",
            "code": "5051794017267"
          },
          "status": "success",
          "messages": []
        },
        {
          "product": {
            "subcode": "OCOM058",
            "codeType": "EAN",
            "code": "12341231234"
          },
          "status": "failed",
          "messages": [
            {
              "message": "No valid product found matching product details.",
              "type": "productDoesNotExist",
              "severity": "error"
            }
          ]
        }
      ],
      "unlinkedProducts": [],
      "status": "success",
      "messages": []
    },
    {
      "assetId": 129389,
      "linkedProducts": [
        {
          "product": {
            "subcode": "OCOM058",
            "codeType": "EAN",
            "code": "5051794017267"
          },
          "status": "failed",
          "messages": [
            {
              "message": "Asset must be public to have product links.",
              "type": "invalidAssetVisibility",
              "severity": "error"
            }
          ]
        }
      ],
      "unlinkedProducts": [],
      "status": "failed",
      "messages": [
        {
          "message": "Asset must be public to have product links.",
          "type": "invalidAssetVisibility",
          "severity": "error"
        }
      ]
    }
  ]
}
```

## POST link/{assetId}
Creates, and publishes, links between the given Enrich asset id and products contained in the request body.

This process will completely replace any previously existing links against the asset, deleting any links not specified in the request body.

This endpoint will return a 200 status response, containing details on whether or not the requested product links were successfully applied.

The asset level operation can have a 'success', 'partialSuccess', or 'failed' status. A partial success is when some product links succeeded, but others failed.

Product level operations can either have a 'success' or 'failed' status.

### Example Request
```
{
	"linkedProducts": [
		{
			"subcode": "OCOM058",
			"codeType": "EAN",
			"code": "5051794017267"
		},
		{
			"subcode": "ANUT059",
			"codeType": "EAN",
			"code": "5900852926433"
		}
	]
}
```


### Example Response
The ``unlinkedProducts`` array contains details of products that were unlinked.

```
{
  "requestId": "6127cdf0-c5a7-4040-9ed6-3ab550f8108e",
  "assetId": 177346,
  "linkedProducts": [
    {
      "product": {
        "subcode": "ANUT059",
        "codeType": "EAN",
        "code": "5900852926433"
      },
      "status": "failed",
      "messages": [
        {
          "message": "Missing permissions to perform this operation.",
          "type": "missingPermissions",
          "severity": "error"
        }
      ]
    },
    {
      "product": {
        "subcode": "OCOM058",
        "codeType": "EAN",
        "code": "5051794017267"
      },
      "status": "success",
      "messages": []
    }
  ],
  "unlinkedProducts": [
    {
        "product": {
            "subcode": "OCOM058",
            "codeType": "EAN",
            "code": "5051794077777"
        },
        "status": "success",
        "messages": []
    }
  ],
  "status": "partialSuccess",
  "messages": []
}
```

# Message Types
The API returns various different message types providing additional information as to the result of the requested operation.
See the swagger schema to see which message types are returned by which endpoints of the API : ![swagger.yaml](swagger.yaml?raw=true "swagger api"). We suggest using https://editor.swagger.io/ to view the schema.

| Message Type | Description  |
|---|---|
| assetOwnerDoesNotExist  | The supplied asset owner does not exist.  |
| assetOwnerDisabled  | The asset owner has been disabled.  |
| InvalidAssetOwnerPermissions  | The asset owner does not have the required permissions to be an owner of this asset. The asset owner must have edit permissions. |
| downloadFailed  | Something went wrong while attempting to download the asset from the provided location. The ``message`` property will include additional detail as to exactly what failed. e.g. The request to download content at url returns error response (400 or 500 http status code). |
| invalidUrl  | The url supplied against the asset is in an invalid format.  |
| invalidFileExtension  | The file extension (on the URL path or resolved from mimetype) is not valid. see ![AcceptedFileExtensions.json](AcceptedFileExtensions.json?raw=true "AcceptedFileExtension") for a list of accepted file extensions.  |
| unableToResolveFileExtension  | No file extension was found on the supplied URL path and it was not possible to resolve a file extension from the content mimetype. See ![AcceptedFileExtensions.json](AcceptedFileExtensions.json?raw=true "AcceptedFileExtension") for a list of accepted file extensions and mime type mappings.  |
| invalidSubcode  | The subcode has been disabled or is not enabled for enrich.   |
| fileSizeLimitExceeded  | The content at the URL has exceeded the file size limit. See 'Request Limits' limits section.  |
| failure  | Unable to process the request due to invalid input. e.g. ReceiptId does not exist, request body too large, empty request body.  |
| internalServerError  | Something went wrong while processing the request. Please contact support.  |
| missingPermissions  | Missing permissions to perform this operation.  |
| downloadTimeout  | Request to download the asset content at the provided url has timed out.  |
| productLinkRemoved  | The following product link has been removed from the asset as part of the update product links operation. |
| duplicateProductCode  | Found duplicate product codes in the ``linkedProducts`` array. |
| invalidAssetVisibility  | The asset is not in a correct state to perform the operation.  |
| duplicateAssetId  |  Found duplciate asset ids in the request. |
| linkedOnConnectTemplate  | Unable to perform operation as the asset and linked products are on a connect template(s).  |
| assetDoesNotExist  |  Provided asset not found. |
| noMatchingProduct  |  No matching product found. |
| invalidProductState  |  Invalid product state - The product must be in Live, Pending Release or Awaiting Approval to be linked to an asset. |
| expiredAsset  |  Unable to perform operation as asset has expired. |
| historicAsset  |  Unable to perform operation as asset has been marked as historic/deleted. |
| userCredentialError | No user_credential header is present or the user_credential is not a valid GUID. |




# Request Restrictions

## URL restrictions
- The content at the URL must be accessible without any need for authentication. For example a leased or SAS URL https://docs.microsoft.com/en-us/azure/storage/common/storage-dotnet-shared-access-signature-part-1
- The URL must be a document path (e.g. https://enrichcontent.storage.com/testing/image.png). If the URL is not a document path an attempt will be made to resolve the file extension from the ``content-type`` header.

## Accepted File Extensions
- See ![AcceptedFileExtensions.json](AcceptedFileExtensions.json?raw=true "AcceptedFileExtension") for a list of accepted file extensions and the mappings used when no file extension is present on the URL.

## Request Limits
- Download Timeout : currently there is an individual upload timeout of 9m 30s, any single asset, in a bulk or single request, exceeding this limit will be marked as failed.
- Request Json Body Size : 1048576 bytes (1MB)
- Max number of assets in a single bulk request : 1000
- Max File Size as content url : 2147483648 bytes (2GB)

Note : There are currently no limits in place around concurrent running requests but we are looking at putting limits in place in future versions, a throttle on concurrent requests or at operation level.

# Versioning Strategy
API Versioning strategy based on https://semver.org/

X.Y.Z | Major.Minor.Patch

Major : Breaking change

Minor : Non breaking change

Patch : Backwards compatible bug fixes
## Authentication ##

Every call on the IbanFirst API must be authenticated, which must be done by adding a custom HTTP header (X-WSSE) with the username and secret provided by the previous request. This section contains detailed instructions on how to create valid headers to execute requests on the API.

**Example of X-WSSE header:**

```js
X-WSSE: UsernameToken
	PasswordDigest="+QpstwYoZeToelOvVaObTdRdEZs=",
	Nonce="d36e3162829ed4c89851497a717f",
	Created="2014-03-20T12:51:45Z"
```
| Field | Description |
|-------|-------------|
| UsernameToken | Authentication method. The X-WSSE header must contain a UsernameToken as we only support token-based authentication. |
| PasswordDigest | Field containing the hashed token which will prove the authenticity of your request. It is essential that you recompute this hash for every request as a hash is only valid for a certain period, and then it expires. |
| Nonce | A random value used to make your request unique so it cannot be replicated by any other unknown party. |
| Created | Field containing the current UTC, GMT, ZULU timestamp (YYYY-MM-DDTHH:MM:SSZ) according to the ISO8601 format, *e.g. 2014-03-20T12:51:45Z*) |

**Computing the Password Digest** 

1.	Get a randomly generated 16 bytes Nonce formatted as 32 hexadecimal characters.
2.	Get the current Created timestamp in ISO-8601 format.
3.	Concatenate the following three values in this order: nonce, timestamp, secret.
4.	Calculate the SHA-1 hash value of the concatenated string, and make sure this value is in binary format! Some languages, like PHP, output hexadecimal hash by default. You may need to use special methods to obtain binary hashes in different languages or even convert byte to hex values by hand (see the sample codes below for more information).
5.	Apply a Base64 encoding to the resulted hash to get the final PasswordDigest value.

**Authenticate the third-party partner**

Albeit using the WSSE security to do request for a user to the API, the third-party partner must authenticate themselves. 

To authenticate, the third-party partner will be granted a partner token, a 16-character hexadecimal string, that will be mandatory to execute requests on the API. 

This partner token must be given by the “X-WSSE-REQUESTED-BY” HTTP header.

```js
X-WSSE-REQUESTED-BY: c6da61fcff03c20b
```

If you do not provide this header doing requests on the API, the request will be rejected.

## API Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Start your company creation project. You get an IBAN in return. |
| [`PUT /companies/{id}/document/{idDoc}`](#put_document) | Upload a document already declared in your project. |
| [`GET /companies/{id}/certificateOfDeposit/`](#getDocuments_certificateDeposit) | Retrieve your certificate of deposit. |
| [`PUT /companies/{id}/releaseDeposit/`](#put_companiesReleaseDeposit) | Ask for your capital to be released. Let's make business now! |
| [`GET /companies/{id}/`](#get_companies) | Retrieve information and status on your company creation project |

## <a id="post_companies"></a> Start a company creation project ##

```
Method: POST 
URL: /companies/
```
You want to create your company? That's great! Let's start your project now, the following data is required to retrieve an IBAN:

On your future company ([Company Creation Data Object](../objects/objects.md#companyCreationData_object)):
* registeredName
* registeredAddress (street, postCode, city, country)
* legalForm
* activityCode
* sharesNumber
* sharesCapital (value, currency)
* percentageRelease

On all founders ([Shareholder Object](../objects/objects.md#shareholder_object)):
* type
* isMainFounder 
* sharesNumber
* email
* isPep
* registeredAddress (street, postCode, city, country)
* registeredIndividual (if type = "individual") (civility, firstName, lastName, nationality, birthDate, birthCity, birthCountry)

By submitting your project, you consider that your project is complete and all documents properly signed.
* We will return an IBAN that each shareholder can use to send their capital deposit. Status is `awaitingFunds` until all funds from shareholder has been correctly collected and matched.
* We will proceed a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated automatically to `pendingKyc` that means that your certificate of deposit has been generated and is ready to be retrieved using the API Route: [`GET /companies/-{id}/certificateOfDeposit/`](#getDocuments_certificateDeposit).
* Next step will be to upload your kbis and ask for the release of the deposit.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| companyCreationDatas | Body | [Company Creation Data Object](../objects/objects.md#companyCreationData_object) | Required | Data regarding your company creation project |
| shareholders | Body | array<[Shareholder Object](../objects/objects.md#shareholder_object)> | Required | Data regarding the shareholding structure behind your company creation project |

**Example of a Call containing all required information at this stage:**
```js
POST /companies/
{
	"companyCreationData": {
		"registeredName": "Pied Pieper Paris",
		"registeredAddress": {
			"street": "42 avenue de la grande armée",
			"postCode": "75017",
			"city": "Paris",
			"country": "FR"
		},
		"activityCode": "334B",
		"legalForm": "SAS",
		"sharesCapital": {
			"value": 100000.00,
			"currency": "EUR"
		},
		"sharesNumber": 100,
		"percentageRelease": 100
	},
	"shareholdingStructure": [
		{
			"type": "Individual",
			"sharesNumber": 50,
			"isMainFounder": true,
			"isPep": false,
			"email": "test@ibanfirst.com",
			"registeredAddress": {
				"street": "4 NEW YORK PLAZA, FLOOR 15",
				"postCode": "75008",
				"city": "Paris",
				"country": "FR"
			},
			"registeredIndividualName": {
				"civility": "M",
				"firstName": "Maxime",
				"lastName": "Champoux",
				"nationality" : "FR",
				"birthDate": "1991-06-25",
				"birthCity": "Pessac",
				"birthCountry" : "FR"
			}			
		},
		{
			"type": "Individual",
			"sharesNumber": 50,
			"isMainFounder": false,
			"isPep": true,
			"email": "test@ibanfirst.com",
			"registeredAddress": {
				"street": "42 avenue de la grande armée",
				"postCode": "75017",
				"city": "Paris",
				"country": "FR"
			},
			"registeredIndividualName": {
				"civility": "M",
				"firstName": "Arnaud",
				"lastName": "Ruppe",
				"nationality" : "FR,BE",
				"birthDate": "1970-01-01",
				"birthCity": "Paris",
				"birthCountry" : "FR"
			}			
		}
	]
}

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../objects/objects.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
{
    "id": "NDgzOTU",
    "registredName": "Pied Pieper Paris",
    "registredAddress": {
        "street": "42 avenue de la grande arm\u00e9e",
        "postCode": "75017",
        "city": "Paris",
        "country": "FR",
        "state": null
    },
    "activityCode": "334B",
    "legalForm": "SAS",
    "authorizedCapital": null,
    "sharesNumber": 100,
    "documents": [],
    "documentsToUpload": [
        {
            "id": "NTM5MTY5",
            "typeName": "BuisnessPlan"
        },
        {
            "id": "NTM5MTcw",
            "typeName": "CompagnyStatusDraft"
        },
        {
            "id": "NTM5MTc2",
            "typeName": "ContractFunderCreasoc"
        }
    ],
    "status": null,
    "shareholdingStructures": [
        {
            "id": "MTgyNTgz",
            "type": "Individual",
            "isMainFounder": true,
            "isPep": false,
            "registeredName": {
                "civility": "M",
                "firstName": "Maxime",
                "lastName": "Champoux",
                "nationality": "FR",
                "birthDate": "1991-06-25",
                "birthCity": "Pessac",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "4 NEW YORK PLAZA, FLOOR 15",
                "postCode": "75008",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcx",
                    "typeName": "Identity"
                },
                {
                    "id": "NTM5MTcy",
                    "typeName": "ProofOfAddress"
                }
            ],
        },
        {
            "id": "MTgyNTg0",
            "type": "Individual",
            "isMainFounder": false,
            "isPep": true,
            "registeredName": {
                "civility": "M",
                "firstName": "Arnaud",
                "lastName": "Ruppe",
                "nationality": "FR,BE",
                "birthDate": "1970-01-01",
                "birthCity": "Paris",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "42 avenue de la grande arm\u00e9e",
                "postCode": "75017",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcz",
                    "typeName": "Identity"
                },
                {
                    "id": "NTM5MTc0",
                    "typeName": "ProofOfAddress"
                }
            ],
        }
    ],
    "accounts": {
        "iban": "BE43914002356001"
    }
}
```

<hr />

### <a id="put_document"></a> Upload a document already declared in your project. ###

```
Method: PUT
URL: /companies/-{id}/document/-{idDoc}/
```

You have already declared documents that you must submit in you company creation project. In return we have sent you and ID for each of those documents.
You can upload those document one by one using this service and must use the ID we have sent you.


**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| idDoc | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this document. |
| documentType | Body | String (60) | Required | The type of document to reference with your company creation project |
| tag | Body | String | Required | The name of the file you want to upload
| file | Body | String | Required | The binary content of the file, encoded with a base64 algorithm. |

**Example:**
```js
PUT /companies/NDgzOTU/document/NTM5MTcx/
{
    "document": {
        "documentType": "Identity",
        "tag": "testIdentity.pdf",
        "file": "DPcCUEQs1hrqSxB3giGY620kQJ1BujlG4rOh+jjvEeW79GpT.....5Oj8dj1wQiKoqyaNGi4cOH51LYvn37k08+WVpaah4"
    }
}

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../objects/objects.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
{
    "id": "NDgzOTU",
    "registredName": "Pied Pieper Paris",
    "registredAddress": {
        "street": "42 avenue de la grande arm\u00e9e",
        "postCode": "75017",
        "city": "Paris",
        "country": "FR",
        "state": null
    },
    "activityCode": "334B",
    "legalForm": "SAS",
    "authorizedCapital": null,
    "sharesNumber": 100,
    "documents": [],
    "documentsToUpload": [
        {
            "id": "NTM5MTY5",
            "typeName": "BuisnessPlan"
        },
        {
            "id": "NTM5MTcw",
            "typeName": "CompagnyStatusDraft"
        },
        {
            "id": "NTM5MTc2",
            "typeName": "ContractFunderCreasoc"
        }
    ],
    "status": null,
    "shareholdingStructures": [
        {
            "id": "MTgyNTgz",
            "type": "Individual",
            "isMainFounder": true,
            "isPep": false,
            "registeredName": {
                "civility": "M",
                "firstName": "Maxime",
                "lastName": "Champoux",
                "nationality": "FR",
                "birthDate": "1991-06-25",
                "birthCity": "Pessac",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "4 NEW YORK PLAZA, FLOOR 15",
                "postCode": "75008",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [
                {
                    "id": "NTM5MTcx",
                    "typeName": "Identity"
                }
            ],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcy",
                    "typeName": "ProofOfAddress"
                }
            ],
        },
        {
            "id": "MTgyNTg0",
            "type": "Individual",
            "isMainFounder": false,
            "isPep": true,
            "registeredName": {
                "civility": "M",
                "firstName": "Arnaud",
                "lastName": "Ruppe",
                "nationality": "FR,BE",
                "birthDate": "1970-01-01",
                "birthCity": "Paris",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "42 avenue de la grande arm\u00e9e",
                "postCode": "75017",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcz",
                    "typeName": "Identity"
                },
                {
                    "id": "NTM5MTc0",
                    "typeName": "ProofOfAddress"
                }
            ],
        }
    ],
    "accounts": {
        "iban": "BE43914002356001"
    }
}
```
<hr />

### <a id="getDocuments_certificateDeposit"></a> Retrieve your certificate of deposit ###

```
Method: GET 
URL: /companies/-{id}/certificateOfDeposit/
```

You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.
This method return two files. It return the PDF file of the certificate of deposit and the audit file as proof of the legal signature

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for your company creation project. |

**Example:**
```js
GET /companies/NDgzOTU/certificateOfDeposit/
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| documents | array<[Document Object]> | The array that contains the two documents you nedd to have a valid certificate deposit |

**Example:** 
```js
{
    documents:[
        "document"{
            "name": 'certificateOfDeposit.pdf',
            "mimeType": "application/pdf" ,
            "link": "http://www.domain.com/an/url/to/curl/file.pdf"
        },
        "document"{
            "name": 'certificateOfDeposit_audit_trail.pdf',
            "mimeType": "application/pdf" ,
            "link": "http://www.domain.com/an/url/to/curl/file.pdf"
        }        
    ]   
}
```
<hr />

### <a id="put_companiesReleaseDeposit"></a> Ask for your capital to be released. Let's make business now. ###

```
Method: PUT
URL: /companies/-{id}/releaseDeposit/
```

Congrats! At this stage, basically your company should be registered. You may use this service to automatically convert your company creation account into a Pro account. Otherwise, just send us the IBAN of the account you want to release your capital (it must be an account at your company's name). 

The following data will be required:
* accountNumber
* registrationNumber
* registrationDate
* Documents : "articleOfAssociationSigned", "proofOfIncorporation", 

Our team will need a quick analysis of the documents you sent before you can enjoy our Pro services. The liberation of your funds must follow in the next 48 hours.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | URL | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| registrationNumber | Body | String (20) | Required | The registration number of the company created. |
| registrationDate | Body | Date | Required | The registration date of the company created. |
| accountNumber | Body | String (20) | Optional | You may use this field if you don't want to release your capital in your iBanFirst Pro account. |
| documents | Body | Array<[Document Object](../conventions/formattingConventions.md#type_document)> | Required | The type of document to reference with your company creation project |

**Example:**
```js
PUT /companies/-NT4edA/certificateIncorporation/
{
	"regitrationNumber": "814455614",
	"regitrationDate": 2017-06-25,
	"accountNumber": "FR76 3000 3009 9500 0503 4076 086",
	"documents": [
		"document": {
			"documentType": "certificateOfIncoraporation",
			"tag": "certificateOfIncorporation.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
		},
		"document": {
			"documentType": "articleOfAssociationSigned",
			"tag": "articleOfAssociationSigned.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
		}
	]
}	
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../objects/objects.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
"companies": {companies},
```
<hr />

### <a id="get_companies"></a> Retrieve information related to a company creation project ###

```
Method: GET 
URL: /companies/-{id}/
```

If you have not implemented our Webhook, you can use this service either this API service to retrieve status and information on your company creation project.

**Parameters:**

| Field | In | Type | Required | Description |
|-------|------|------|----------|-------------|
| id | URL |[ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for your company creation project. |

**Example:**
```js
GET /companies/NDgzOTU/
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../objects/objects.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
{
    "id": "NDgzOTU",
    "registredName": "Pied Pieper Paris",
    "registredAddress": {
        "street": "42 avenue de la grande arm\u00e9e",
        "postCode": "75017",
        "city": "Paris",
        "country": "FR",
        "state": null
    },
    "activityCode": "334B",
    "legalForm": "SAS",
    "authorizedCapital": null,
    "sharesNumber": 100,
    "documents": [],
    "documentsToUpload": [
        {
            "id": "NTM5MTY5",
            "typeName": "BuisnessPlan"
        },
        {
            "id": "NTM5MTcw",
            "typeName": "CompagnyStatusDraft"
        },
        {
            "id": "NTM5MTc2",
            "typeName": "ContractFunderCreasoc"
        }
    ],
    "status": null,
    "shareholdingStructures": [
        {
            "id": "MTgyNTgz",
            "type": "Individual",
            "isMainFounder": true,
            "isPep": false,
            "registeredName": {
                "civility": "M",
                "firstName": "Maxime",
                "lastName": "Champoux",
                "nationality": "FR",
                "birthDate": "1991-06-25",
                "birthCity": "Pessac",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "4 NEW YORK PLAZA, FLOOR 15",
                "postCode": "75008",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcx",
                    "typeName": "Identity"
                },
                {
                    "id": "NTM5MTcy",
                    "typeName": "ProofOfAddress"
                }
            ],
        },
        {
            "id": "MTgyNTg0",
            "type": "Individual",
            "isMainFounder": false,
            "isPep": true,
            "registeredName": {
                "civility": "M",
                "firstName": "Arnaud",
                "lastName": "Ruppe",
                "nationality": "FR,BE",
                "birthDate": "1970-01-01",
                "birthCity": "Paris",
                "birthCountry": "FR"
            },
            "registeredCompagny": null,
            "registeredIndividualCountry": "FR",
            "email": "test@ibanfirst.com",
            "percentage": null,
            "numberOfParts": 50,
            "regsiteredAddress": {
                "street": "42 avenue de la grande arm\u00e9e",
                "postCode": "75017",
                "city": "Paris",
                "country": "FR",
                "state": null
            },
            "documents": [],
            "documentsToUpload": [
                {
                    "id": "NTM5MTcz",
                    "typeName": "Identity"
                },
                {
                    "id": "NTM5MTc0",
                    "typeName": "ProofOfAddress"
                }
            ],
        }
    ],
    "accounts": {
        "iban": "BE43914002356001"
    }
}

```
<hr />


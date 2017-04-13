## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Start a company creation project |
| [`PUT /companies/-{id}/iban`](#put_companiesIban) | Ask for an IBAN |
| [`PUT /companies/-{id}/projectComplete`](#put_companiesComplete) | Ask for a certificate of deposit |
| [`GET /companies/-{id}/certificateDeposit/`](#getDocuments_certificateIncorporation) | Retrieve your certificate of deposit |
| [`POST /companies/-{id}/certificateIncorporation`](#post_companiesCertificateIncorporation) | Upload your Kbis |
| [`POST /companies/-{id}/releaseDeposit`](#post_companiesReleaseDeposit) |  Release the deposit and enjoy your iBanFirst account  |
| [`PUT /companies/-{id}/`](#put_companies) | Update information relative to a company creation project|
| [`GET /companies/-{id}/`](#get_companies) | Retrieve infromation relative to a company creation project |
| [`DELETE /companies/-{id}/`](#delete_companies) | Delete a company creation project |
| [`POST /companies/-{id}/documents/`](#putDocuments_companies) | Submit documents relative to a company creation project |
| [`DELETE /companies/-{id}/documents/-{id}`](#putDocuments_companies) | Delete documents relative to a company creation project |
| [`PUT /companies/-{id}/shareholder/-{id}/`](#put_companies) | Update information relative to a shareholder |
| [`GET /companies/-{id}/shareholder/-{id}/`](#put_companies) | Retrieve information relative to a shareholder |
| [`PUT /companies/-{id}/shareholder/-{id}/documents/`](#put_companies) | Update document relative to a shareholder |

<hr />

## <a id="post_companies"></a> Start a company creation project ##

```
Method: POST 
URL: /companies/
```
You want to create your company? That's great! Let's start you project now, only a minimum of information is needed to open a file:

On your future company ([Company Creation Data Object](../objects/objects.md#companyCreationData_object)):
* registeredName
* registeredAddress

On the main founder ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type 
* isMainFounder
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* shareholdingStructure (if type = "corporate" and for all shareholders on the 2 level owning +25%)
* email

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| companyCreationData | [Company Creation Data Object](../objects/objects.md#companyCreationData_object) | Required | Standard information on the projet and the future activity of the company. |
| shareholdingStructure | Array<[Shareholder Object](../objects/objects.md#shareholder_object)> | Required | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on the future company. 

**Example:**
```js
POST /companies/
{
    "companyCreationData": {
    	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
    		"postCode": "75008",
    		"city": "Paris",str
   		"country": "FR",
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"type": "Individual",
		"isMainFounder": true,
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"email": "mch@ibanfirst.com",
	},
    },			
},
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

### <a id="put_companiesIban"></a> Ask for an IBAN ###

```
Method: PUT 
URL: /companies/-{id}/iban
```
Ok well, at this stage we will require the following datas & documents:

On your future company ([Company Creation Data Object](../objects/objects.md#companyCreationData_object)):
* legalForm
* registeredName
* registeredAddress
* activityCode
* sharesNumber
* sharesCapital
* liberatedPercentage
* document = "openingAccountAgreement", "projectArticleOfAssociation"

On the main founder ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholders on the 2 level owning +25%)

On the other individual founders ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

By submitting your project, you will have in return an IBAN that you can share with the co-founders for collecting the deposit of each one.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation. |

**Example of a Call containing anll required information at this stage:**
```js
PUT /companies/NT4edA/iban
{
    "companyCreationDatas": {
	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
		"postCode": "75008",
		"city": "Paris",
		"country": "FR",
	},
	"activityCode": 334B,
	"legalForm": "EURL",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountAgreement",
			"id": "Rocket Startup - Opening Account Agreement",
		},
		"document": {
			"type": "projectArticleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
			},
		},
	},
	
	"shareholder": {
		"id": "WZ4edA",
		"sharesNumber": 10000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualCountry": FR,
		"registeredIndividualNationality": "FR",
		"birthDate": 25-06-1991,
		"birthCountry": "FR",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "John Doe - CNI",
			},
		},
	},
	"shareholder": {
		"type": "Corporate",
		"sharesNumber": 40000.00,
		"legalForm": "EURL",
		"registeredCorporateAddress": "My Holding",
		"isMainFounder": false,
		"email": "myHolding@email.com",
		"registeredCorporateAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": {
			"document": {
				"type": "certificateOfIncorporation",
				"id": "KBIS - myHolding",
			},
			"document": {
				"type": "articleOfAssociation",
				"id": "KBIS - myHolding",
			},
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"sharespercentage": 100%,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualCountry": FR,
				"registeredIndivdualNationality": "France",
				"birthDate": 25-06-1991,
				"birthCountry": "France",
				"isPep": true,
				"documents": {
					"document": {
						"type": "idProof",
						"id": "Maxime Champoux - CNI",
					},
				},
			},
		},
	},
    },		
},

```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| companies | [Companies Object](../objects/objects.md#companies_object) | Your up-to-date company creation project description |

**Example:** 
```js
"companies": {companies},
```

### <a id="put_companiesComplete"></a> Ask for a certificate of deposit ###

```
Method: PUT 
URL: /companies/-{id}/projectComplete
```
At this stage, we will require additional data and documents:

On your future company ([Company Creation Data Object](../objects/objects.md#companyCreationData_object)):
* legalForm
* registeredName
* registeredAddress
* activityCode
* sharesNumber
* sharesCapital
* liberatedPercentage
* document = "openingAccountAgreement", "projectArticleOfAssociation", "articleOfAssociation, "businessPlan"

On the main founder ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry if type = "corporate")
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

On the other founders ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredIndividualCountry (or registeredCorporateCountry depending on the type)
* registeredIndividualName (or registeredCorporateName depending on the type)
* is Pep (if type = "individual")
* document = "IDProof" (or "certificateOfIncorporation" if type = "corporate")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

By submitting your project, you consider that your project is complete and we will proceed to a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated to "certificate of deposit ready" and you will be able to retrieve your certificate using the request ```GET ../companies/-{id}/document/certificateDeposit```.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |


**Example of a Call containing anll required information at this stage:**
```js
PUT /companies/NT4edA/projectComplete
{
    "companyCreationDatas": {
	"registeredName": "Rocket Startup",
	"registeredAddress": {
		"street": "4 NEW YORK PLAZA, FLOOR 15",
		"postCode": "75008",
		"city": "Paris",
		"country": "FR",
	},
	"activityType": 334B,
	"legalForm": "EURL",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountAgreement",
			"id": "Rocket Startup - Opening Account Agreement",
		},
		"document": {
			"type": "projectArticleOfAssociation",
			"id": "Rocket Startup - Projets de Statuts",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "Maxime Champoux - CNI",
			},
		},
	},
	"shareholder": {
		"id": "WZ4edA",
		"sharesNumber": 10000.00,
		"type": "Individual",
		"isMainFounder": true,
		"email": "mch@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualCountry": FR,
		"registeredIndivdualNationality": "France",
		"birthDate": 25-06-1991,
		"birthCountry": "France",
		"isPep": true,
		"documents": {
			"document": {
				"type": "idProof",
				"id": "John Doe - CNI",
			},
		},
	},
	"shareholder": {
		"sharesNumber": 40000.00,
		"type": "Corporate",
		"legalForm": "EURL",
		"isMainFounder": false,
		"email": "myHolding@email.com",
		"registeredCorporateAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": {
			"document": {
				"type": "certificateOfIncorporation",
				"id": "KBIS - myHolding",
			},
			"document": {
				"type": "articleOfAssociation",
				"id": "KBIS - myHolding",
			},
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"sharespercentage": 100%,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualCountry": FR,
				"registeredIndivdualNationality": "France",
				"birthDate": 25-06-1991,
				"birthCountry": "France",
				"isPep": true,
				"documents": {
					"document": {
						"type": "idProof",
						"id": "Maxime Champoux - CNI",
					},
				},
			},
		},
	},
    },		
},

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

### <a id="post_companiesCertificateIncorporation"></a> Upload your Kbis ###

```
Method: POST 
URL: /companies/-{id}/CertificateIncorporation
```

At this stage, basically your company should be registered. Congratulation! Therefore, we will require the registration information on your company:
* Document: certificate of incorporation, signed article of association, status: uploaded.
* Registration number
* Registration date

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| registrationNumber | String (20) | Required | The registration number of the company created. |
| documentType | [Document Type](../conventions/formattingConventions.md#type_document) | Required | The type of document to reference with your company creation project |
| documentUrl | [Document Url](../conventions/formattingConventions.md#type_url) | Required | The url of document to reference with your company creation project |

**Example:**
```js
POST /companies/NT4edA/certificateIncorporation/
{
	"regitrationNumber": 814455614,
	"regitrationDate": 25-06-2017,
	"documentType": "certificateIncorporation",
	"documentUrl": "https://ph-files.imgix.net/07d80ab9-a5e1-4d70-ae85-ddf51ff79e1f?auto=format&auto=compress&codec=mozjpeg&cs=strip&fit=crop&w=120&h=120",
	
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

### <a id="post_companiesReleaseDeposit"></a> Release the deposit and enjoy your iBanFirst account ###

```
Method: POST 
URL: /companies/-{id}/releaseDeposit
```

As soon as we have finalized the due diligence on your company, you can ask for the release of the deposit by signing and uploading your Opening account agreement. An iBanFirst user access to your account will be automatically created. The main founder will receive this user access by email.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| documentType | [Document Type](../conventions/formattingConventions.md#type_document) | Required | The type of document to reference with your company creation project |
| documentUrl | [Document Url](../conventions/formattingConventions.md#type_url) | Required | The url of document to reference with your company creation project |


**Example:**
```js
POST /NT4edA/releaseDeposit
{
	"documentType": "openingAccountAgreement",
	"documentUrl": "https://ph-files.imgix.net/07d80ab9-a5e1-4d70-ae85-ddf51ff79e1f?auto=format&auto=compress&codec=mozjpeg&cs=strip&fit=crop&w=120&h=120",
}
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| user | [User Object](../objects/objects.md#user_object) | Submit documents relative to a company creation project |

**Example:** 
```js
"companies": {companies},
```
<hr />

### <a id="#putDocuments_transactions"></a> Attach a document to a transaction ###

```
Method: PUT 
URL: /transaction/-{id}/proofOfTransaction
```
We may ask you to provide a proof of transaction under specific terms. You can anticipate our request and send us your invoice or the ID of the beneficiary to avoid any request from us and fully automate your payment process.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| documentType | [Document Type](../conventions/formattingConventions.md#type_document) | Required | The type of document to submit for a transaction |
| tag | String (60) | Required | The name of the document to be attached. |
| file | String (60) | Required | The name of the document to be attached. |



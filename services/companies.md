## Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Start a company creation project |
| [`PUT /companies/-{id}/iban/`](#put_companiesIban) | Ask for an IBAN |
| [`PUT /companies/-{id}/projectComplete/`](#put_companiesComplete) | Ask for a certificate of deposit |
| [`GET /companies/-{id}/certificateOfDeposit/`](#getDocuments_certificateDeposit) | Retrieve your certificate of deposit |
| [`PUT /companies/-{id}/certificateIncorporation/`](#post_companiesCertificateIncorporation) | Upload your Kbis |
| [`PUT /companies/-{id}/releaseDeposit/`](#post_companiesReleaseDeposit) | Ask for your capital to be released and enjoy your iBanFirst account |
| [`GET /companies/-{id}/`](#get_companies) | Retrieve information relative to a company creation project |
| [`DELETE /companies/-{id}/`](#delete_companies) | Delete a company creation project |
| [`PUT /companies/-{id}/documents/`](#putDocuments_companies) | Submit document related to a company creation project |
| [`PUT /companies/-{id}/shareholder/-{id}/documents/`](#put_companies) | Submit document related to a shareholder |

## <a id="post_companies"></a> Start a company creation project ##

```
Method: POST 
URL: /companies/
```
You want to create your company? That's great! Let's start your project now, only a minimum of information is needed to open a file:

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
URL: /companies/-{id}/iban/
```
Ok well, at this stage we will require the following datas & documents:

On your future company ([Company Creation Datas Object](../objects/objects.md#companyCreationDatas_object)):
* registeredName
* legalForm
* registeredAddress
* activityCode
* sharesNumber
* sharesCapital
* liberatedPercentage
* document = "openingAccountContract", "articleOfAssociationDraft"

On the main founder ([Shareholder Object](../objects/objects.md#shareholder_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredAddress 
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* registeredIndividualNationality (if type = "individual")
* document = "proofOfIdentity" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholders on the 2 level owning +25%)

On the other individual founders ([Shareholder Object](../objects/objects.md#shareholder_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredAddress 
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* registeredIndividualNationality (if type = "individual")
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

By submitting your project, you will have in return an IBAN that you can share with the co-founders for collecting the capital.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation. |

**Example of a Call containing all required information at this stage:**
```js
PUT /companies/NT4edA/iban/
{
    "companyCreationDatas": {
	"registeredName": "Pied Pieper Paris",
	"legalForm": "SAS",
	"registeredAddress": {
		"street": "42 avenue de la grande armée",
		"postCode": "75017",
		"city": "Paris",
		"country": "FR",
	},
	"activityCode": "334B",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"liberatedPercentage": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountContract",
			"id": "Rocket Startup - Opening Account Agreement.pdf",
		},
		"document": {
			"type": "articleOfAssociationDraft",
			"id": "Rocket Startup - Projets de Statuts.pdf",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "individual",
		"isMainFounder": true,
		"birthDate": 1991-06-25,
		"birthCountry": "FR",
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		}
		"email": "test@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredAddress": {
			"street": "7 rue barye",
			"postCode": "75017",
			"city": "Paris",
			"country": "FR"
		}
		"registeredIndividualNationality": "FR",
		"isPep": false,
		"documents": {
			"document": {
				"type": "proofOfIdentity",
				"id": "Maxime Champoux - CNI.png",
			},
		},
	},
	"shareholder": {
		"id": "WZ4edA",
		"sharesNumber": 10000.00,
		"type": "individual",
		"isMainFounder": false,
		"email": "test@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualNationality": "FR",
		"isPep": false,
	},
	"shareholder": {
		"id": "XY4edA",
		"type": "corporate",
		"sharesNumber": 40000.00,
		"legalForm": "EURL",
		"registeredCorporateAddress": "My Holding",
		"isMainFounder": false,
		"email": "myHolding@email.com",
		"registeredAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"type": "individual",
				"sharespercentage": 100.00,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualNationality": "FR",
				"isPep": true,
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
URL: /companies/-{id}/projectComplete/
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
* registeredAddress
* registeredIndividualName (or registeredCorporateName if type = "corporate")
* registeredIndividualNationality (if type = "individual")
* document = "proofOfIdentity" (or "certificateOfIncorporation" if type = "corporate")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* phoneNumber
* is Pep (if type = "individual")
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

On the other founders ([Shareholding Structure Object](../objects/objects.md#shareholdingStructure_object)):
* type
* isMainFounder = true
* sharesNumber
* email
* registeredAddress 
* registeredIndividualName (or registeredCorporateName depending on the type)
* registeredIndividualNationality (if type = "individual")
* birthDate (if type = "individual")
* birthCountry (if type = "individual")
* is Pep (if type = "individual")
* document = "proofOfIdentity" (or "certificateOfIncorporation" and "articleOfAssociationSigned" if type = "corporate") and "mandateShareholder"
* shareholdingStructure (if type = "corporate" and for all shareholder on the 2 level owning +25%)

By submitting your project, you consider that your project is complete and we will proceed to a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated to "certificate of deposit ready" and you will be able to retrieve your certificate using the request ```GET ../companies/-{id}/document/certificateDeposit```.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |


**Example of a Call containing all required information at this stage:**
```js
PUT /companies/NT4edA/iban/
{
    "companyCreationDatas": {
	"registeredName": "Pied Pieper Paris",
	"legalForm": "SAS",
	"registeredAddress": {
		"street": "42 avenue de la grande armée",
		"postCode": "75017",
		"city": "Paris",
		"country": "FR",
	},
	"activityCode": "334B",
	"sharesCapital": {
		"value": 100000.00,
		"currency": "EUR",
	}
	"sharesNumber": 100.00,
	"liberatedPercentage": 100.00,
	"documents": {
		"document": {
			"type": "openingAccountContract",
			"id": "Rocket Startup - Opening Account Agreement.pdf",
		},
		"document": {
			"type": "articleOfAssociationDraft",
			"id": "Rocket Startup - Projets de Statuts.pdf",
		},
	},
    },
    "shareholdingStructure": {
    	"shareholder": {
		"id": "XV4edA",
		"sharesNumber": 50000.00,
		"type": "individual",
		"isMainFounder": true,
		"birthDate": 1991-06-25,
		"birthCountry": "FR",
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		}
		"email": "test@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "Maxime",
			"lastName": "Champoux",
		},
		"registeredAddress": {
			"street": "7 rue barye",
			"postCode": "75017",
			"city": "Paris",
			"country": "FR"
		}
		"registeredIndividualNationality": "FR",
		"isPep": false,
		"documents": {
			"document": {
				"type": "proofOfIdentity",
				"id": "Maxime Champoux - CNI.png",
			},
		},
	},
	"shareholder": {
		"id": "WZ4edA",
		"sharesNumber": 10000.00,
		"type": "individual",
		"isMainFounder": false,
		"birthDate": 1991-06-25,
		"birthCountry": "FR",
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		}
		"email": "test@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualNationality": "FR",
		"isPep": false,
		"documents": {
			"document": {
				"type": "proofOfIdentity",
				"id": "John Doe - CNI.png",
			},
			"document": {
				"type": "mandateShareholder",
				"id": "John Doe - PowerOfAttorney.png",
			},
		},
	},
	"shareholder": {
		"id": "XY4edA",
		"type": "corporate",
		"sharesNumber": 40000.00,
		"legalForm": "EURL",
		"registeredCorporateAddress": "My Holding",
		"isMainFounder": false,
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		}
		"email": "myHolding@email.com",
		"registeredAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": {
			"document": {
				"type": "certificateOfIncorporation",
				"id": "KBIS - myHolding.pdf",
			},
			"document": {
				"type": "articleOfAssociationSigned",
				"id": "KBIS - myHolding.pdf",
			},
			"document": {
				"type": "mandateShareholder",
				"id": "myHolding - PowerOfAttorney.png",
			},
		},
		 "shareholdingStructure": {
			"shareholder": {
				"id": "WE4edA",
				"type": "individual",
				"birthDate": 1991-06-25,
				"birthCountry": "FR",
				"sharespercentage": 100.00,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualNationality": "FR",
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

### <a id="getDocuments_certificateDeposit"></a> Retrieve your certificate of deposit ###

```
Method: GET 
URL: /companies/-{id}/certificateOfDeposit/
```

You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |

**Example:**
```js
GET /companies/NT4edA/certificateOfDeposit/
```

**Returns:**

| Field | Type | Description |
|-------|------|-------------|
| documentType | [Document Type](../conventions/formattingConventions.md#type_document) | The type of document to retrieve. `certificateOfDeposit` |
| tag | String (60) | Will be `certificateOfDeposit.pdf` |
| file | String | The binary content of the file, encoded with a base64 algorithm. |

**Example:** 
```js
{
    "documentType": "certificateOfDeposit",
    "tag": "certificateOfDeposit.pdf",
    "file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",

}
```
<hr />

### <a id="post_companiesCertificateIncorporation"></a> Upload your Kbis ###

```
Method: PUT
URL: /companies/-{id}/certificateIncorporation/
```

At this stage, basically your company should be registered. Congratulation! Therefore, we will require the registration information on your company:
* Document = "certificateOfIncorporation", "articleOfAssociationSigned"
* registrationNumber
* registrationDate

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| registrationNumber | String (20) | Required | The registration number of the company created. |
| registrationDate | Date | Required | The registration date of the company created. |
| documents | Array<[Document Object](../conventions/formattingConventions.md#type_document)> | Required | The type of document to reference with your company creation project |

**Example:**
```js
PUT /companies/NT4edA/certificateIncorporation/
{
	"regitrationNumber": 814455614,
	"regitrationDate": 25-06-2017,
	"documents": {
		"document": {
			"documentType": "certificateOfDeposit",
			"tag": "certificateOfDeposit.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
		},
		"document": {
			"documentType": "articleOfAssociationSigned",
			"tag": "articleOfAssociationSigned.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
		},
	},
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

### <a id="post_companiesReleaseDeposit"></a> Release the deposit and enjoy your iBanFirst account ###

```
Method: PUT 
URL: /companies/-{id}/releaseDeposit/
```

As soon as we have finalized the due diligence on your company, you can ask for the release of the deposit by signing and uploading your Opening account agreement. An iBanFirst user access to your account will be automatically created. The main founder will receive his user access by email.

We will threrefore require the following document :
* Document = "finalOpeningAccountContract"

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| documents | Array<[Document Object](../conventions/formattingConventions.md#type_document)> | Required | The type of document to reference with your company creation project |


**Example:**
```js
PUT /companies/NT4edA/certificateIncorporation/
{
	"documents": {
		"document": {
			"documentType": "finalOpeningAccountContract",
			"tag": "finalOpeningAccountContract.pdf",
			"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
		},
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





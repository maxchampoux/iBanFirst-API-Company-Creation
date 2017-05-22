## API Routes ##

| Route | Description |
|-------|-------------|
| [`POST /companies/`](#post_companies) | Start your company creation project. You get an IBAN in return. |
| [`PUT /companies/-{id}/releaseDeposit/`](#put_companiesReleaseDeposit) | Ask for your capital to be released. Let's make business now. |
| [`PUT /companies/-{id}/document/`](#out_document) | Upload a document already declared in your project. |
| [`GET /companies/-{id}/`](#get_companies) | Retrieve information related to a company creation project |
| [`GET /companies/-{id}/certificateOfDeposit/`](#getDocuments_certificateDeposit) | Retrieve your certificate of deposit |

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
* shares
* sharesCapital (value, currency)
* liberated
* documents ("openingAccountAgreement", "articleOfAssociationDraft").

On all founders ([Shareholder Object](../objects/objects.md#shareholder_object)):
* type
* isMainFounder 
* shares
* email
* is Pep (if type = "individual")
* registeredAddress (street, postCode, city, country)
* registeredIndividual (if type = "individual") (civility, firstName, lastName, nationality, birthDate, birthCity, birthCountry)
* registeredCorporate (if type = "corporate"):
* documents = "proofOfIdentity" (if type = "individual")
* documents = "certificateOfIncorporation", "articleOfAssociationSigned" (if type = "corporate")
* phoneNumber
* shareholdingStructure (if type = "corporate" and for all shareholder on the level 2 owning 25% and +)

By submitting your project, you consider that your project is complete and all documents properly signed.
* We will return an IBAN that each shareholder can use to send their capital deposit. Status is `awaitingFunds` until all funds from shareholder has been correctly collected and matched.
* We will proceed a due diligence review of your project, the shareholders and the presence of the right deposits. When we are fine, your project status will be updated automatically to `pendingKyc` that means that your certificate of deposit has been generated and is ready to be retrieved using the API Route: [`GET /companies/-{id}/certificateOfDeposit/`](#getDocuments_certificateDeposit) or the Webhook:[`toto`](#webhook).
* Next step will be to upload your kbis and ask for the release of the deposit.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |


**Example of a Call containing all required information at this stage:**
```js
POST /companies/
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
	},
	"shares": 100,
	"liberated": 100.00,
	"documents": [
		"document": {
			"type": "openingAccountContract",
			"tag": "Pied Pieper - openingAccountContract.pdf",
		},
		"document": {
			"type": "articleOfAssociationDraft",
			"tag": "Pied Pieper - articleOfAssociationDraft.pdf",
		},
	],
    },
    "shareholdingStructure": {
    	"shareholder": {
		"shares": 50000,
		"type": "individual",
		"isMainFounder": true,
		"birthDate": 1991-06-25,
		"birthCountry": "FR",
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		},
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
		},
		"registeredIndividualNationality": "FR",
		"isPep": false,
		"documents": [
			"document": {
				"type": "proofOfIdentity",
				"tag": "Maxime Champoux - CNI.png",
			],
		},
	},
	"shareholder": {
		"id": "WZ4edA",
		"shares": 10000,
		"type": "individual",
		"isMainFounder": false,
		"birthDate": 1991-06-25,
		"birthCountry": "FR",
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		},
		"email": "test@ibanfirst.com",
		"registeredIndividualName": {
			"civility": "M",
			"firstName": "John",
			"lastName": "Doe",
		},
		"registeredIndividualNationality": "FR",
		"isPep": false,
		"documents": [
			"document": {
				"type": "proofOfIdentity",
				"tag": "John Doe - CNI.png",
			},
			"document": {
				"type": "mandateShareholder",
				"tag": "John Doe - PowerOfAttorney.png",
			},
		],
	},
	"shareholder": {
		"id": "XY4edA",
		"type": "corporate",
		"shares": 40000,
		"legalForm": "EURL",
		"registeredCorporateAddress": "My Holding",
		"isMainFounder": false,
		"phoneNumber": {
		    "countryCode": "+33",
		    "phoneNumber": "671738257",
		},
		"email": "myHolding@email.com",
		"registeredAddress": {
			"street": "1 rue de l'université",
			"postCode": "75006",
			"city": "Paris",
			"country": "France",
		},
		"documents": [
			"document": {
				"type": "certificateOfIncorporation",
				"tag": "KBIS - myHolding.pdf",
			},
			"document": {
				"type": "articleOfAssociationSigned",
				"tag": "KBIS - myHolding.pdf",
			},
			"document": {
				"type": "mandateShareholder",
				"tag": "myHolding - PowerOfAttorney.png",
			},
		],
		 "shareholdingStructure": [
			"shareholder": {
				"id": "WE4edA",
				"type": "individual",
				"birthDate": 1991-06-25,
				"birthCountry": "FR",
				"ownership": 100.00,
				"registeredIndividualName": {
					"civility": "M",
					"firstName": "Maxime",
					"lastName": "Champoux",
				},
				"registeredIndividualNationality": "FR",
				"isPep": true,
				"documents": [
					"document": {
						"type": "idProof",
						"tag": "Maxime Champoux - CNI",
					},
				],
			},
		],
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

### <a id="get_companies"></a> Retrieve information related to a company creation project ###

```
Method: GET 
URL: /companies/-{id}/
```

If you have not implemented our Webhook, you can use this service either this API service to retrieve status and information on your company creation project.

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |

**Example:**
```js
GET /companies/-NT4edA/
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
GET /companies/-NT4edA/certificateOfDeposit/
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

### <a id="put_companiesReleaseDeposit"></a> Ask for your capital to be released. Let's make business now. ###

```
Method: PUT
URL: /companies/-{id}/releaseDeposit/
```

Congrats! At this stage, basically your company should be registered and you should have opened an IBAN account for this company. Maybe using our [Onboarding Service](../toto). Congratulation! Therefore, we will require the registration information on your company:
* accountNumber
* registrationNumber
* registrationDate
* Documents : "articleOfAssociationSigned", "proofOfIncorporation", 

**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| registrationNumber | String (20) | Required | The registration number of the company created. |
| registrationDate | Date | Required | The registration date of the company created. |
| documents | Array<[Document Object](../conventions/formattingConventions.md#type_document)> | Required | The type of document to reference with your company creation project |

**Example:**
```js
PUT /companies/-NT4edA/certificateIncorporation/
{
	"regitrationNumber": "814455614",
	"regitrationDate": 2017-06-25,
	"accountNumber": "R76 3000 3009 9500 0503 4076 086",
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
		},
	],
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

### <a id="put_document"></a> Upload a document already declared in your project. ###

```
Method: PUT
URL: /companies/-{id}/document/
```

You have already declared documents that you must submit in you company creation project. In return we have sent you and ID for each of those documents.
You can upload those document one by one using this service and must use the ID we have sent you.


**Parameters:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | Required | The internal reference for this company creation project. |
| documentType | String (60) | Required | The type of document to reference with your company creation project |
| file | String | Required | The binary content of the file, encoded with a base64 algorithm. |

**Example:**
```js
PUT /companies/-NT4edA/document/
{
"document": {
	"id": "aB4edA",
	"documentType": "certificateOfIncoraporation",
	"tag": "certificateOfIncorporation.pdf",
	"file": "iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAMAAAAoLQ9TAAAABGdBTUEAANbY1E9YMgAAABl0RVh0U29mdHdhcmUAQWRvYmUgSW1hZ2VSZWFkeXHJZTwAAAGAUExURQxS1ISawgBGyebt+VZ6vmGK1miV58bO3O3u8gRJykV31E170brM7RNSyXuRukJ64jNkvl2H1Xmh6wFK0fT19+vw++rs8QFGxlt/xOLm7KOwylSB01l7urbB1LW/0lJ/0py25vDw8+ju+oucv9PZ4yJezUh40oOo7zJpzSljzV6I1lmE1Ep50e7z/PDx9Ky4zfb2+JOt3FF+0hlc2AlLxjxvzUFptEd406+60EZ73NTf9EhxvWGP5XeOuixm0z9x0HeQvSxlzVB90xZZ1h5d0unr7+7w85qx3s/V4Stgwo+x8bnC1b/I2Vp6tliC0ViD1H6ZzF6G0UyD6GSAtVB+1Chm2drf5yVgzZy68kh931F6xlR/zuDp+tbg9N/o+l6Bw8HJ2iFk4DRw4YCWv0p601KF5V6P6T9puW+Ku4qewnCIt3iOuE980WSL0iVk2Stfvixo1jlz3Vt9vl5+uk11wPPz9VyDzAhO0MnQ3S5lyYeg0FB90aG+80l40Nzg6P///xIhGr0AAACAdFJOU/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////8AOAVLZwAAAPdJREFUeNpiqAcBPRVvWQ8OMJMBiEXkbIvjJWSy9PUgAmLKRYFarKzafix8kiABA2UJQW1WHh5BeemobA6ggByLv7Q8a6yVIDc3d4lUPUMpX7SRUXUOqxa3jo5abbArQ5ivEze3jqCCQoiGo2Z4OjsDO0sKl72GuaiSpri4OFO+BQO7tSqvibgqM7MLExB4WjDUmXGKM3HaKSkVlAsLCwtUMIhk8HIyuFiKikbmGTMwyIgx1MsKOIcW2ujqsvEnJVZKgRzGaMqfKhQXo54WxOXgBnZ6ZhmbekSNl1BusiTUc7KMAYbuVYwWUM8BgaJKgo8KxPsAAQYAJwc98FQAQqUAAAAASUVORK5CYII=",
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




# API Objects & Lists

* [Companies Object](#companies_object)
* [Company Creation Datas Object](#companyCreationDatas_object)
* [Shareholder Object](#shareholder_object)
* [Status List](#status_list)
* [Founders Object](#founder_object)
* [Address Object](#address_object)
* [Account Object](#account_object)
* [Phone Object](#phone_object)
* [Individual Name Object](#individualName_object)
* [Amount Object](#amount_object)

## Details ##

#### <a id="companies_object"></a> Companies Object ####

My object to follow where I am in the company creation process.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](#type_id) | The IF code identifying the company to be created. |
| status | String (60) | The status of your company creation project. The full list of status is accessible in the [Status List](#status_list)  |
| companyCreationDatas | [Company Creation Datas Object](#companyCreationDatas_object) | Information, documents regarding the company you want to create. |
| shareholdingStructures | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on your company creation project. |
| account | [Account Object](#account_object) | The IBAN account that has been opened for the purpose of your company creation project. |

**Example:**
```js
"companies": {
    	 "id": NT4edA,
	 "status": "awaitingFunds",
	 "companyCreationDatas": {companyCreationDatas}
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
			"id": "PK4edA",
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
	"account": {
		"currency": EUR,
		"accountNumber": "FR914516981638516313513",
		"holderName": "Rocket Startup [En cours de création]",
		"financialMovements": null,
	},     
},
```
<hr />

#### <a id="companyCreationDatas_object"></a> Company Creation Datas Object ####

Specific information required for opening a company creation file.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| legalForm | [Legal Form](#legalForm) | The legal form of the company to be created. "SASU", "EURL"  |
| registeredName | String(100) | The legal name of the company to be created. |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| activityCode | [NAFID](#NAF) | The code identifying the type of business of the company to be created. |
| sharesNumber | Value | The number of shares to be issued. |
| sharesCapital | [Amount Object](#amount_object) | The amount in shareholding capital as mentionned in the status. |
| liberatedPercentage | Value | The percentage of shareholding capital to be released when the company is created. "20", "50" or "100". |
| commercialName | String(100) | The commercial name of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| documents | Array<[Document Object](#document_object)> | The required documents for creating a company. |

**Example:**

```js
"companyCreationDatas": {
    "registeredName": "DJPAD",
    "tag":"null",
    "registeredAddress": {address},
    "commercialAddress": {address},
    "activityCode":"6201Z",
    "legalForm":"SARL unipersonnelle",
    "authorizedCapital":{amount},
    "documents": [
    	"document": {
		"type": "article of association",
		"tag": "NameOfTheDocument",
	}
	"document": {
		"type": "kbis",
		"tag": "NameOfTheDocument",
	}
    ]
}
```

<hr />


#### <a id="companyCreationDatas_object"></a> Company Creation Datas Object ####

Specific information required for submitting a company creation file.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | String (5) | Your business activity as registered with local authorities. To see a full list of state code, please refer to [this site](https://www.insee.fr/fr/information/2406147). |
| legalForm | [Legal Form](#legalForm) | The legal form of the company to be created.. |
| authorizedCapital | [Amount Object](#amount_object)  | The amount in shareholding capital as mentionned in the status. |
| documents | Array<[Document Object](#document_object)> | The required documents for creating a company. |

**Example:**

```js
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
```

<hr />

#### <a id="companyRegistrationDatas_object"></a> Company Registration Datas Object ####

Additional information required for releasing "capital social".

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the entity opening the account. |
| registrationDate | [Date](#type_date) | The legal date of creation of the entity. |

**Example:**

```js
"companyRegistrationDatas": {
    "registeredParentNumber": "	814455614",
    "registrationDate":"2015-11-04",
}
```

<hr />

#### <a id="shareholder_object"></a> Shareholder Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| ownership | Percentage | The pourcentage of ownership the shareholder has witha  direct company |
| shareholder | [Company Shareholding Datas Object](#companyShareholdingDatas_object) or [Individual Shareholding Datas Object](#individualShareholdingDatas_object) | Specific data that is required on shareholder. |


**Example:**

```js
"shareholderStructures": [
    "shareholder": {
    	"ownership": 20%,
	"shareholderData": {companyShareholdingDatas},
    },
    "shareholder": {
    	"ownership": 80%,
	"shareholderDatas": {individualShareholdingDatas},
    },
}
```

<hr />

#### <a id="status_list"></a> Status List ####

Here is the list of status you may encounter while using the iBanFirst APi.

**Object resources:**

| Status | Description |
|-------|-------------|
| subscribing | Welcome to your company creation journey! A few document will be required to go to the next step and obtain you IBAN. |
| awaitingFunds | When your project is ready and all required document has been filled and signed, we return you this status together with you IBAN. |
| pendingKyc | When all funds has been collected and matched, you may ask for your certificate of deposit. This call triggered a KYC process on our side. |
| pendingInformation | While we are making our KYC process and reviewing your projects. We may ask you some more documents or information. Your project will not move to the enxt step until you provide the required document or information. |
| rejectedKyc | Something went wrong with your application, you are not compliant with our acceptance criterion. Please apply again or contact your account manager. |
| awaitingIncorporation | When certificate of deposit is available, you can use it to incorporate your company with the appropriate local authorities. |
| checkKbis | When certificate of deposit is available, you can use it to incorporate your company with the appropriate local authorities. |
| finalized | Here we are, you company is created and you capital has just been released to the bank account you just opened. Congrats! |


#### <a id="companyShareholdingDatas_object"></a> Company Shareholding Datas Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredName | String(100) | The legal name of the company to be created. |
| commercialName | String(100) | The commercial name of the company to be created. |
| tag | String(100) | The customized name of the company to be created. (Will only be used internally). |
| registeredAddress | [Address Object](#address_object) | The registered address of the company to be created. |
| commercialAddress | [Address Object](#address_object) | The commercial address of the company to be created. |
| activityCode | [NAFID](../conventions/formattingConventions.md#NAF) | The code identifying the type of business of the company to be created. |
| legalForm | [Legal Form](../conventions/formattingConventions.md#legalForm) | The legal form of the company to be created.. |

<hr />

#### <a id="individualShareholdingdataDatas_object"></a> Individual Shareholding Datas Object ####

This object shows the shareholder ownership and detailed information.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [Phone Object](#phone_object)  | The phone number of the entity. |
| position | [Position Object](#position_object)  | The position of the entity. |


**Example:**

```js
"contact": {
    "id": ND4ue2,
    "registeredNumber": "81445561400010",
    "registeredName": {individualName},
    "tag":"null",
    "address": {address},
    "birthDate":"1980-11-04",
    "phoneNumber":{phone},
    "position":{position},
}
```
<hr />

#### <a id="founder_object"></a> Founder Object ####

The Founder object.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| registeredNumber | String(100) | The unique legal identifier of the contact. |
| registeredCountry| String(100) | The registering country of the contact. |
| registeredName | [Individual Name Object](#individual_name_object) | The individual name of the contact. |
| tag | String(100) | The customized name of the contact. |
| address | [Address Object](#address_object) | The contact address. |
| birthDate | [Date](#type_date) | The birthdate of the contact. |
| phoneNumber | [Phone Object](#phone_object)  | The phone number of the entity. |
| position | [Position Object](#position_object)  | The position of the entity. |

<hr />

#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account (can be either an Iban or an account number). |
| holderBank | [ID](#type_id) | The recipient bank details, holding the account. |
| holder | [Holder Object](#holder_object) | The recipient details, owner of the account. If the company is in pending creation, then [pending creation] will be added aside the owner name. |

**Example:**

```js
"account": {
    "currency": "EUR",
    "tag": "My wallet account EUR",
    "accountNumber": "516981638516313513",
    "correspondantBank":{correspondentBank}
    "holderBank":{beneficiaryBank}
    "holder":{beneficiary}
}
```

<hr />


#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street1 | String(255) | The street for the address described. |
| street2 | String(255) | The continuation street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| state | String(2) | The state code for the address described. This field could be required if the country use a state system, like United States or Canada. To see a full list of state code, please refer to [this site](http://www.mapability.com/ei8ic/contest/states.php). |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "4 NEW YORK PLAZA, FLOOR 15",
	"postCode": "10004",
	"city": "NEW YORK",
	"state": "NY",
	"country": "US"
}
```

<hr />

#### <a id="phone_object"></a> Phone Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| countryCode | String(4) | Numeric country code. Optional +, followed by 1-3 digits. |
| phoneNumber | String(15) | Country code and phone number. |

**Example:**

```js
"phone": {
    "countryCode": "+33",
    "phoneNumber": "81445561400010",
}
```

<hr />

#### <a id="individualName_object"></a> Individual Name Object ####

When a phone number is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| first | String(35) | The individual's first name. Truncated after the first 35 characters. |
| middle | String(35) | The individual's middle name. Truncated after the first 35 characters. |
| last | String(35) | The individual's last name. Truncated after the first 35 characters. |

**Example:**

```js
"phone": {
    "first": "John",
    "last": "Doe",
}
```

<hr />

#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | [Quoted Decimal](../conventions/formattingConventions.md#type_quoteddecimal) | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "GBP"
}
```

<hr />


#### <a id="document_object"></a> Document Object ####

The list of document that can be submitted for a shareholder/ individual.


**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| idProof | [Id Types](#idTypes_object)  | The url where the file is stored. |
| addressProof | [Proof of Address Object](#addressProof_object)  | The url where the file is stored. |

**Example:**

```js
TBD
```


# API Objects & Lists

* [Companies Object](#companies_object)
* [Company Creation Datas Object](#companyCreationDatas_object)
* [Shareholder Object](#shareholder_object)
* [Status List](#status_list)
* [Address Object](#address_object)
* [Account Object](#account_object)
* [Registered Individual Object](#registeredIndividual_object)
* [Registered Corporate Object](#registeredCorporate_object)
* [Amount Object](#amount_object)
* [Document Object](#document_object)
* [Document List](#document_list)

## Details ##

#### <a id="companies_object"></a> Companies Object ####

The main object in my company creation project. Status is automatically updated when there is a progress in my project.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the company to be created. |
| status | String (60) | The status of your company creation project. The full list of status is accessible in the [Status List](#status_list)  |
| registredName | [Company Creation Datas Object](#companyCreationDatas_object) | Information, documents regarding the company you want to create. |
| registredAddress | [Company Creation Datas Object](#companyCreationDatas_object) | Information, documents regarding the company you want to create. |
| activityCode | String (6) | Information, documents regarding the company you want to create. |
| legalForm | String | Information, documents regarding the company you want to create. |
| activityCode | String (6) | Information, documents regarding the company you want to create. |
| authorizedCapital | Float | Information, documents regarding the company you want to create. |
| sharesNumber | Integer | Information, documents regarding the company you want to create. |
| documents | Array<Document> | Information, documents regarding the company you want to create. |
| documentToUpload | Array<DocumentToUpload> | Information, documents regarding the company you want to create. |
| shareholdingStructures | Array<[Shareholder Object](#shareholder_object)> | The regulatory list of shareholders, part of the Ultimate Beneficiary Owners that must be identified as part as our Compliance procedure on your company creation project. |
| account | [Account Object](#account_object) | The IBAN account that has been opened for the purpose of your company creation project. |



**Example:**
```js
{
    "id": "NDgzOTU",
    "status": "registration",
    "registredName": "Pied Pieper Paris",
    "registredAddress": { address },
    "activityCode": "334B",
    "legalForm": "SAS",
    "authorizedCapital": null,
    "sharesNumber": 100,
    "documents": [{ document },{ document }],
    "documentsToUpload": [{ documentToUpload },{ documentToUpload }],
    "shareholdingStructures":[{ shareholder },{ shareholder }],
    "account":{ account }
},
```
<hr />

#### <a id="shareholder_object"></a> Shareholder Object ####

This object shows the shareholder ownership and detailed information. We gave you below two detailed examples in case you have a corporate or an individual as shareholder.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| type | String (10) | It can be `Individual` or `Corporate`. |
| isMainFounder | Boolean | Indicates who is introducing the project among the project. It can be `true` or `false`. You can only have on Main Founder. |
| sharesNumber | Integer | The number of shares that belong to the shareholder. |
| isPep | Boolean | True if the shareholder is politically exposed |
| email | string (60) | Dedicated email of the shareholder. We may use this email to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have an email format. |
| registeredAddress | [Address Object](#address_object) | The registered address of the shareholder. |
| registeredIndividuaName | [Registered Individual Object](#registeredIndividual_object) | The registered information of the shareholder when type is `individual`. |
| registeredCorporate | [Registered Corporate Object](#registeredIndividual_object) | The registered information of the shareholder when type is `corporate`. |
| documents | Array<[Document Object](#document_object)> | The required documents related to this shareholder. |
| phoneNumber | [Phone](../conventions/formattingConventions.md#type_phone) | Dedicated phone number of the shareholder. We may use this number to send personal information about the company in project. We are also checking the format of the field and return an error if we don't have the right format. |
| ownership | Float (5) | Percentage of shares a shareholder level 2 owns from a shareholder level 1 with type `corporate`. |

**Example 1: individual shareholder**

```js
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
```

**Example 2: corporate shareholder**

```js
"shareholder": {
	"id": "PK4edA",
	"type": "Corporate",
	"shares": 40000,
	"isMainFounder": false,
	"phoneNumber": "+33671738257",
	"email": "myHolding@email.com",
	"registeredCorporate": {registeredCorporate},
	"registeredAddress": {address},
	"documents": [{document}...],
	"shareholdingStructure": [{shareholder},{...}],
]
```

<hr />

#### <a id="registeredIndividual_object"></a> Registered Individual Object ####

Specific information when the shareholder is an individual.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| civility | String(3) | It can be `M` or `Mme`. |
| firstName | String(35) | The individual's first name. Truncated after the first 35 characters. |
| lastName | String(35) | The individual's last name. Truncated after the first 35 characters. |
| nationality | String | The two-letters abbreviation for the country where the shareholder is registered if type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166).|
| birthDate | Date | The birth date of the shareholder when type is `individual`. `YYYY-MM-DD` |
| birthCity | String(35) | The indidual's birth city. Truncated after the first 35 characters. |
| birthCountry | String (2) | The two-letters abbreviation for the country where the shareholder is born when type is `individual`, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166). |
| isPep | Boolean | You indicates if the shareholder is legally recognized as a [PEP](https://en.wikipedia.org/wiki/Politically_exposed_person). `true` or `false`. |

**Example:**

```js
"registeredIndividual": {
	"civility": "M",
	"firstName": "Maxime",
	"lastName": "Champoux",
	"nationality": "FR",
	"birthDate": 1991-06-25,
	"birthCity": "Pessac",
	"birthCountry": "FR",
	"isPep": false,
},	
```
<hr />

#### <a id="status_list"></a> Status List ####

Here is the list of status you may encounter while using the iBanFirst API.

**Document resources:**

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

<hr />

#### <a id="account_object"></a> Account Object ####

When an Account is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency of the account. |
| tag |  String(50) | Custom reference of the account. |
| accountNumber | String(40) | The code specifying the account - will be an IBAN. |
| holderBank | String (11) | The code identifying iBanFirst or BIC/SWIFT code. |
| holder | String(50) | The recipient details, owner of the account. If the company is in pending creation, then [pending creation] will be added aside the owner name. |

**Example:**

```js
"account": {
    "currency": "EUR",
    "tag": "My payment account EUR",
    "accountNumber": "BE169816385163133",
    "holderBank": "FXBBBEBBXXX",
    "holderName": "Pied Pieper Paris [société en cours de formation]",
}
```

<hr />

#### <a id="address_object"></a> Address Object ####

When an address is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| street | String(255) | The street for the address described. |
| postCode | String(15) | The ZIP/Post code for the address described. |
| city | String(35) | The city for the address described. |
| country | String(2) | The two-letters abbreviation for the country, following the [ISO-3166](http://fr.wikipedia.org/wiki/ISO_3166) for the address described. |

**Example:**

```js
"address": {
	"street": "42 avenue de la grande armée",
	"postCode": "75017",
	"city": "Paris",
	"country": "US"
}
```

<hr />


#### <a id="amount_object"></a> Amount Object ####

When an amount of currency is specified as part of a JSON body, it is encoded as an object with the following fields:

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| value  | Float | The quantity of the currency. |
| currency | [Currency](../conventions/formattingConventions.md#type_currency) | The three-digit code specifying the currency related to the amount. At this moment, only EUR is available |

**Example:**

```js
"amount": {
	"value": "10000.00",
	"currency": "EUR"
}
```

<hr />

#### <a id="document_object"></a> Document Object ####

The list of document that can be submitted for a shareholder/ individual.

**Object resources:**

| Field | Type | Description |
|-------|------|-------------|
| id | [ID](../conventions/formattingConventions.md#type_id) | The IF code identifying the document. |
| typeName | String | The type of your document as you must used it with the corresponding `PUT /companies/-{id}/document/` calls. |

**Example:**

```js
{
    "id": "aB4edA",
    "typeName": "Identity"
}
```

<hr />

#### <a id="document_list"></a> Document List ####

Here is the list of documents you may encounter while using the iBanFirst APi.

**Document resources for individuals:**

| Name | Description |
|-------|-------------|
| Identity | An official document confirming your identity. |
| ProofOfAddress | An official document confirming your address. |

**Document resources for compagny creation:**
| Name | Description |
|-------|-------------|
| BuisnessPlan | An official document confirming your identity. |
| CompagnyStatusDraft | An official document confirming your address. |
| ContractFunderCreasoc | An official document confirming the agreement for the main funder. |


<hr />







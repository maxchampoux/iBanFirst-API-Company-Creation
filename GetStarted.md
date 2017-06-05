## GETTING STARTED WITH IBANFIRST REST COMPANY CREATION API ##

#### 1. Create your company project ####

Please use the following service to create your company project: [`POST /companies/`](/services/companies.md#post_companies)

This service will return an IBAN for the deposit of the "Share Capital" of your future company.
We will require some mandatory documents and let you some time to gather the full KYC until next step.

When your project is complete, i.e. we have received all transfers, documents and information needed, we will proceed to the analysis of your project. This can take up to 48 hours before your certificate of deposit can be generated. In case you have implemented a webhook, you will be notified as soon as this certificate is available. In other cases, you may call our API to get the status and retrieve the document when available.

PS: documents file must be provided with a specific call : [`PUT /companies/-{id}/document/`](/services/companies.md#out_document)

#### 2. Retrieve your certificate of deposit ####

You can use either this API service [`GET /companies/-{id}/certificateDeposit/`](/services/companies.md#getDocuments_certificateIncorporation) or a tailor-made webhook to retrieve your certificate of deposit.

#### 3. 	Open a Pro account  ####

You now have your certificate of incorporation, that's great! It means that your company now exists and that you will be able to open a Pro account and start your activity. 

Please use the following service to open a Pro account :

[`POST /onboard/`](/services/companies.md#post_onboard)

#### 4. 	Ask for your capital to be released  ####

You are there! Use this call to have access to your account en enjoy your iBanFirst account!

can enjoy you account after a quick verification from our side.
 [`PUT /companies/-{id}/releaseDeposit/`](#put_companiesReleaseDeposit)
 

#### XX. Know where you are in your company creation project ####

[`GET /companies/-{id}/`](/services/companies.md#get_companies) 

You want to know if all shareholders has deposited funds to the IBAN? You can use this service for having more information on the status of your project.

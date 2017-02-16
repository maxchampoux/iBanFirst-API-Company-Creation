## GETTING STARTED WITH IBANFIRST REST COMPANY CREATION API ##

#### 1. Create your company project ####

Please use the following service to create your company project:
[`POST /companies/`](../objects/formattingConventions.md#post_companies)

#### 2. Ask for an IBAN ####

[`PUT /companies/-{id}/iban`](#put_companiesIban)
This service will return an IBAN for the deposit of the "Capital Social" of your future company.
We will require the signature, ID and power of attorney of all founders at this stage. 

#### 3. Ask for a certificate of deposit ####

[`PUT /companies/-{id}/projectComplete`](#put_companiesComplete)
Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, the analysis of your project and the generation of the certificate can take up to 48 hours. In case you have implemented a webhook, you will be notified as soon as this certificate is available. In other cases, you may call our API to get the status and retrieve the document when available.

#### 4. Retrieve your certificate of deposit ####

 [`GET /companies/-{id}/document/certificateDeposit/`](#getDocuments_certificateIncorporation)
You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.

#### 5. Upload your Kbis ####

 [`PUT /companies/-{id}/document/certificateIncorporation`](#put_companiesCertificateIncorporation) 
Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, you must upload the official certificate of incorporation.

#### 6. Ask for the release of the deposit ####

[`PUT /companies/-{id}/releaseDeposit`](#put_companiesReleaseDeposit)
Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, you must upload the official certificate of incorporation.

#### XX. Know where you are in your company creation project ####

[`GET /companies/-{id}/`](#get_companies) 
You want to know if all shareholders has deposited funds to the IBAN? You can use this service for having more information on the status of your project.

#### XX. Update information in your project ####

 [`PUT /companies/-{id}/`](#put_companies)
You have new information you want to share with us. That's great! Updating your project shall move it to the next step.

#### XX. Delete your project ####

[`DELETE /companies/-{id}/`](#delete_companies)
We are so sad you are doing that. See you next time!

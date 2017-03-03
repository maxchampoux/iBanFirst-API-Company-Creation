## GETTING STARTED WITH IBANFIRST REST COMPANY CREATION API ##

#### 1. Create your company project ####

Please use the following service to create your company project:

[`POST /companies/`](/services/companies.md#post_companies)

#### 2. Ask for an IBAN ####

[`PUT /companies/-{id}/iban`](/services/companies.md#put_companiesIban)

This service will return an IBAN for the deposit of the "Capital Social" of your future company.
We will require some mandatory documents and let you some time to gather the full KYC when needed.

#### 3. Ask for a certificate of deposit ####

[`PUT /companies/-{id}/projectComplete`](/services/companies.md#put_companiesComplete)

Until your project is not complete, the following service will return errors mentionning the missings information.
When your project is complete, the analysis of your project and the generation of the certificate can take up to 48 hours. In case you have implemented a webhook, you will be notified as soon as this certificate is available. In other cases, you may call our API to get the status and retrieve the document when available.

#### 4. Retrieve your certificate of deposit ####

 [`GET /companies/-{id}/document/certificateDeposit/`](/services/companies.md#getDocuments_certificateIncorporation)
 
You can use either this API service, a FTP server or a tailor-made webhook to retrieve your certificate of deposit.

#### 5. Upload your Kbis ####

 [`POST /companies/-{id}/certificateIncorporation`](/services/companies.md#post_companiesCertificateIncorporation) 
 
You have you certificate of Incorporation, that's great! It means that you company now exist and that you can enjoy you account after a quick verification from our side.

#### 6. Release the deposit and enjoy your iBanFirst account  ####

[`POST /companies/-{id}/releaseDeposit`](/services/companies.md#post_companiesReleaseDeposit)

You are there! Use this call to have access to your account en enjoy your iBanFirst account!

#### XX. Know where you are in your company creation project ####

[`GET /companies/-{id}/`](/services/companies.md#get_companies) 

You want to know if all shareholders has deposited funds to the IBAN? You can use this service for having more information on the status of your project.

#### XX. Update information in your project ####

 [`PUT /companies/-{id}/`](/services/companies.md#put_companies)
 
You have new information you want to share with us. That's great! Updating your project shall move it to the next step.

#### XX. Delete your project ####

[`DELETE /companies/-{id}/`](/services/companies.md#delete_companies)

We are so sad you are doing that. See you next time!

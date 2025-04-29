## FormBuilder Pro SaaS Description

FormBuilder Pro SaaS is a cloud-based platform designed to empower users to create, manage, and publish online forms efficiently. Unlike traditional form builders with drag-and-drop interfaces, this system emphasizes a programmatic approach to form creation via an API that accepts descriptive text. This allows for automated form generation and integration with various systems.

## FormBuilder Pro SaaS Core Functionality

**API-Driven Form Definition:** Users can send descriptive text payloads to the FormBuilder Pro API, outlining the form's structure, fields, validation rules, and basic appearance hints. The API leverages AI to parse this text and generate a standardized JSON schema representing the form.
**Form Syntax Retrieval:** Client systems can retrieve the generated JSON schema and associated rendering code (React Web Component) via an API endpoint using a unique form identifier.
**Client-Side Rendering:** The platform provides a pre-built React Web Component that client websites can embed using a custom HTML tag. This component dynamically renders the form based on the retrieved JSON schema.
**Form Submission Handling:** When a user submits a form on a client website, the submission data is sent to the FormBuilder Pro API. The API validates the data against the defined schema.
**Automated Email Validation:** If the form definition includes email fields with validation rules, the system automatically sends a validation email to the submitted address.
**User Management:** The system handles user registration, login, and account management.
**Subscription and Billing:** The platform offers both free (limited submissions) and paid subscription plans with additional features and higher submission limits. Users can purchase credits for paid features like iframe embedding (potentially as a secondary option) and higher usage.
**User Panel:** A web-based user panel allows users to view and manage their created forms and submitted responses (with history limitations based on their subscription).
**WordPress Plugin (Optional):** An optional WordPress plugin provides a more integrated way for WordPress users to create and embed forms.
**API Key Authentication:** All API requests for accessing and managing forms and submissions are authenticated using API keys associated with user accounts.
**QR Code Generation:** For published forms on paid plans, the system generates and stores QR codes for easy access.

## FormBuilder Pro SaaS Existing Scenario

A developer needs to embed a contact form with specific fields and validation rules on their company website (which is not built with React).
The developer logs into the FormBuilder Pro SaaS user panel and navigates to the form creation section.
Instead of using a visual builder, they enter a detailed text description of the contact form, specifying fields like "Name (required)", "Email (required, email format)", "Message (optional)", and a submit button. They might also include basic appearance hints like "primary color: blue".
The FormBuilder Pro SaaS API receives this text and uses an integrated AI model to understand the request and generate a JSON schema representing the form structure and validation rules.
The API returns a unique formId to the developer.
The developer then retrieves the rendering code (the React Web Component) and the JSON schema using the formId via another API endpoint.
On their company website, the developer includes the necessary JavaScript to register the FormBuilder Pro Web Component.
They then add a custom HTML tag (e.g., <form-builder data-form-id="{formId}"></form-builder>) to the desired location on their contact page, passing the retrieved formId.
When a visitor fills out and submits the form on the company website, the data is sent to the FormBuilder Pro API.
The API validates the submitted data based on the JSON schema. If the email field has an invalid format, an error is returned to the client-side Web Component for display. If the email field requires validation, an email with a validation link is automatically sent to the provided address.
If the submission is successful and valid, the data is stored in the FormBuilder Pro SaaS database.
The developer can view the submissions in their FormBuilder Pro SaaS user panel (within the limitations of their free or paid plan).
To access submission data programmatically or manage the form via the API, the developer uses their API key for authentication in the request headers.

## Entities With Descriptions

**USER:** Represents an individual who has registered and can log in to the FormBuilder Pro SaaS platform.
**ACCOUNT:** Represents a specific account or workspace owned by a user, associated with billing, subscription plans, and the forms created within it.
**API_KEY:** Represents an authentication key assigned to a user, used to authorize API requests made by the user's applications or systems.
**FORM_DEFINITION:** Stores the complete definition of a form, including the original descriptive text provided to the AI, the generated JSON schema, and the reference to the rendering code.
**FORM_PUBLICATION:** Contains information about how a specific form definition has been published, including its public link and QR code path (if applicable).
**FORM_SUBMISSION:** Represents a single instance of a user submitting a published form.
**SUBMISSION_DATA:** Stores the individual field values submitted as part of a FORM_SUBMISSION.
**EMAIL_VALIDATION:** Tracks the status and details of email validation processes triggered by form submissions.
**PLAN:** Defines the different subscription plans offered by the platform, including their features and pricing.
**CREDIT_TRANSACTION:** Records all transactions related to the purchase or usage of credits by a user's account.

## Entities Properties And Types

**USER:**
* `userId` (Integer, PK)
* `email` (String, Unique, Not Null)
* `cellnumber` (Number)
* `passwordHash` (String, Not Null)
* `firstName` (String)
* `lastName` (String)
* `registrationDate` (DateTime, Not Null)
* `role` (String)

---

**ACCOUNT:**
* `accountId` (Integer, PK)
* `planId` (Integer, FK)
* `accountName` (String)
* `creationDate` (DateTime, Not Null)
* `creditBalance` (Decimal)
* `lastBillingDate` (DateTime)

---

**API_KEY:**
* `apiKey` (String, PK, Unique, Not Null)
* `userId` (Integer, FK, Not Null)
* `creationDate` (DateTime, Not Null)
* `lastUsed` (DateTime)
* `description` (String)
* `expiration` (DateTime)

---

**FORM_DEFINITION:**
* `formId` (Integer, PK)
* `accountId` (Integer, FK, Not Null)
* `title` (String)
* `descriptionTextAI` (String)
* `formSchemaJSON` (JSON, Not Null)
* `renderingCode` (String, Not Null)
* `creationDate` (DateTime, Not Null)
* `lastUpdated` (DateTime)
* `submissionEmail` (String)

---

**FORM_PUBLICATION:**
* `publicationId` (Integer, PK)
* `formId` (Integer, FK, Not Null)
* `isPublished` (Boolean, Not Null)
* `publishLink` (String, Unique)
* `qrCodePath` (String)

---

**FORM_SUBMISSION:**
* `submissionId` (Integer, PK)
* `formId` (Integer, FK, Not Null)
* `submissionDate` (DateTime, Not Null)
* `ipAddress` (String)

---

**SUBMISSION_DATA:**
* `dataId` (Integer, PK)
* `submissionId` (Integer, FK, Not Null)
* `fieldName` (String, Not Null)
* `fieldValue` (String)

---

**EMAIL_VALIDATION:**
* `validationId` (Integer, PK)
* `submissionId` (Integer, FK, Not Null)
* `email` (String, Not Null)
* `validationToken` (String, Unique)
* `isValidated` (Boolean, Not Null, Default: False)
* `sentAt` (DateTime)
* `validatedAt` (DateTime)

---

**PLAN:**
* `planId` (Integer, PK)
* `planName` (String, Unique, Not Null)
* `price` (Decimal)
* `freeSubmissions` (Integer)
* `fullHistory` (Boolean, Not Null, Default: False)
* `iframeEmbed` (Boolean, Not Null, Default: False)
* `apiAccess` (Boolean, Not Null, Default: True)

---

**CREDIT_TRANSACTION:**
* `transactionId` (Integer, PK)
* `accountId` (Integer, FK, Not Null)
* `transactionDate` (DateTime, Not Null)
* `amount` (Decimal, Not Null)
* `description` (String)

**(PK = Primary Key, FK = Foreign Key, Unique = Unique Constraint, Not Null = Not Null Constraint, Default = Default Value)**

## Entities Relations And Dependent Entities

**Here are the relationships between the entities:**

* **USER** to **ACCOUNT**: One-to-many (One user can own multiple accounts). ACCOUNT is dependent on USER for its existence (ownership).
* **USER** to **API_KEY**: One-to-many (One user can have multiple API keys). API_KEY is dependent on USER.
* **ACCOUNT** to **FORM_DEFINITION**: One-to-many (One account can have many form definitions). FORM_DEFINITION is dependent on ACCOUNT.
* **ACCOUNT** to **CREDIT_TRANSACTION**: One-to-many (One account can have many credit transactions). CREDIT_TRANSACTION is dependent on ACCOUNT.
* **PLAN** to **ACCOUNT**: One-to-many (One plan can be subscribed to by many accounts). ACCOUNT is dependent on PLAN.
* **FORM_DEFINITION** to **FORM_PUBLICATION**: One-to-one (One form definition has one publication record). FORM_PUBLICATION is dependent on FORM_DEFINITION.
* **FORM_DEFINITION** to **FORM_SUBMISSION**: One-to-many (One form definition can have many submissions). FORM_SUBMISSION is dependent on FORM_DEFINITION.
* **FORM_SUBMISSION** to **SUBMISSION_DATA**: One-to-many (One submission can have many submitted data entries). SUBMISSION_DATA is dependent on FORM_SUBMISSION.
* **FORM_SUBMISSION** to **EMAIL_VALIDATION**: One-to-zero-or-one (One submission can trigger at most one email validation record). EMAIL_VALIDATION is dependent on FORM_SUBMISSION.

**Dependent Entities:**

Based on the relationships, the dependent entities are:

* ACCOUNT (dependent on USER, PLAN)
* API_KEY (dependent on USER)
* FORM_DEFINITION (dependent on ACCOUNT)
* CREDIT_TRANSACTION (dependent on ACCOUNT)
* FORM_PUBLICATION (dependent on FORM_DEFINITION)
* FORM_SUBMISSION (dependent on FORM_DEFINITION)
* SUBMISSION_DATA (dependent on FORM_SUBMISSION)
* EMAIL_VALIDATION (dependent on FORM_SUBMISSION)

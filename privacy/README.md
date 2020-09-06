# Privacy

If in any case you believe a project will not meet privacy requirements, **it is your professional responsibility** to raise this issue with your project manager and lead engineer.

_(Note that the following are guidelines, not legal advice.)_

## Design for Users, Design for Privacy

You should approach building projects with the right to privacy in mind. This is both a legal requirement in some jurisdictions as well as being an important part of respecting the user.

Designing for privacy means collecting the minimum amount of data you need, ensuring you really do need that data, and ensuring the user is informed about how that data is used. Access to data also needs to be restricted to the minimum set of users who need to access this data, and should only be stored for as long as necessary.

When client requirements dictate collecting data, you should ensure that these requirements do not conflict with privacy regulations in the relevant jurisdictions. If you notice a problem (or suspect that one exists), it is important to communicate this to the project team and the client, as these requirements may need to be changed.

## Basic Guidelines

* **Collect as little as necessary**: Don’t collect data unless you need it.
* **Protect user data with access controls**: Only allow the people or systems that need the data to access it.
* **Store user data in one place, and refer to it by ID**: This allows users to easily edit their personal data.
* **Allow users to be deleted**: User accounts may be deleted. Keep track of where you use a user’s ID and allow this to be deleted.
* **Anonymise data when exporting**: If you’re getting a copy of production data for a staging or development site, user data must be anonymised. Note that user IDs count as personal data, and must also be anonymised.

## European General Data Protection Regulation (GDPR)

The EU GDPR is one of the most important regulations, as it affects both HM as a European company, as well as [affecting European end users](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/application-regulation/who-does-data-protection-law-apply_en). Everything we produce must follow GDPR regulations.

The [key rules](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/principles-gdpr/what-data-can-we-process-and-under-which-conditions_en) should be kept in mind when developing.

These restrictions apply to all [personal data](https://ec.europa.eu/info/law/law-topic/data-protection/reform/what-personal-data_en), which includes any data that relates to an identifiable individual. This also includes data linked to a pseudonym if the data can in any way be tied to the individual.

There are several important parts to the GDPR which affect how we work.

### Grounds for Collecting Data

To collect data on users, you need to have grounds for processing that data. For projects we work on, this should typically fall into one of the following areas:

* **Legitimate interests**: Data can be collected if there’s a legitimate need for it, but should be done in the least intrusive way. This must not seriously impact the user’s rights. Where possible, data should fit into legitimate interests.
* **Consent**: Data can be collected if the user has consented to it. This is the best grounds to meet, as it provides an explicit confirmation of the use of users data.
* **Contractual obligation**: Data that is necessary to fulfill a contract. For example, shipping and billing information for ecommerce.

(While other grounds exist, they are not likely to apply to the projects we work on, or will be clearly defined by the client if they are.)

Deciding on a basis for collecting data should happen as a conversation with the client. Follow this guidance to provide feedback to the client on complying with the law. **It is your responsibility** to ensure these requirements are being met by the client. If you believe there is an issue, raise it with the project manager and lead engineer.

Where possible, legitimate interests should be used as the grounds for collecting data. When following the “collect as little data as necessary” principle, the only data we gather should easily fit into these grounds. These grounds must meet [specific criteria](https://ico.org.uk/for-organisations/guide-to-the-general-data-protection-regulation-gdpr/lawful-basis-for-processing/legitimate-interests/): the legitimacy must be **identified**, the processing must be **justified**, and the use must be **balanced** against the user’s rights. Generally, if the data is required for us to perform a task, and the user fully expects this use, then legitimate interests can be used as the grounds.

For optional features (for example, email marketing, or further analytics), explicit consent should be gained from the user. This must follow [specific rules](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/grounds-processing/when-consent-valid_en) for gaining the consent of the user. In particular, it must be **freely given** (not tied to another, required action), it must be **explicit** via a positive act (such as an opt-in checkbox; opt-out checkboxes are not valid consent), and it must be able to be **withdrawn**.

It is important to carefully identify whether a feature is truly necessary. A feature cannot be claimed as necessary simply to work around the GDPR rules. If a user would not reasonably expect a feature, then it must instead use explicit consent.

These should be taken into consideration when building features that require consent. The ability to opt-out at a later date must exist (such as an unsubscribe link), and any features like this must be clearly siloed and tied directly to the consent. The motivation for collecting the data, and exactly how we use it must be conveyed to the user, which means the client must fully understand how the data is used.

There are [further, stricter rules](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/sensitive-data/under-what-conditions-can-my-company-organisation-process-sensitive-data_en) for [sensitive data](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/sensitive-data/what-personal-data-considered-sensitive_en). This typically will not apply to us, as we do not usually collect this data or facilitate its collection. If this sort of data collection is requested, **it is your responsibility** to ensure these requirements are being met by the client. If you believe there is an issue, raise it with the project manager and lead engineer.

### Right to Access Data and Right to Data Portability

Users have the right to [access data stored about themselves](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/dealing-citizens/what-personal-data-and-information-can-individual-access-request_en). This includes:

* A confirmation of whether or not a user’s data is being collected
* Providing a copy of their personal data
* Providing information about how that data is used

When building sites, any time personal data is collected, used, or processed, the use and intent should be recorded. This allows clients to faithfully provide this information to users.

Separately, users have a right to data portability. This allows users to request a copy of their personal data, which includes any data they have provided, as well as any generated data about the user (depending on the ground for collecting that data). This data must be provided in a “structured, commonly used and machine-readable format”. (It should not infridge on the privacy of others, however.)

In the context of our projects, this would typically entail providing data in a format like CSV, JSON, or XML. The format provided should be the [most appropriate one possible](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/dealing-citizens/can-individuals-ask-have-their-data-transferred-another-organisation_en).

Companies are required to provide this data [as quickly as possible](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/dealing-citizens/how-should-requests-individuals-exercising-their-data-protection-rights-be-dealt_en) (with a time limit of one month). This means that systems should be designed from the start to enable or provide data access and portability, and clients should be able to provide this data to their users.

### Right to Erasure and Right to Restriction

Users have the [right to be forgotten](https://ico.org.uk/for-organisations/guide-to-the-general-data-protection-regulation-gdpr/individual-rights/right-to-erasure/); that is, their data should be erased. This occurs in two main ways: the data is no longer needed, or the user has [withdrawn their consent](https://ec.europa.eu/info/law/law-topic/data-protection/reform/rules-business-and-organisations/legal-grounds-processing-data/grounds-processing/what-if-somebody-withdraws-their-consent_en). There are some cases in which this may not be applicable, such as for legal purposes, or when this would conflict with freedom of expression.

Systems should be designed to allow clients to delete data associated with users. This deletion process should cover all data stored for that user. This should either be designed for direct use by users, or can be used by the company, provided processes are in place to allow the user to request deletion.

If the user data is sent anywhere else (e.g. support systems), the deletion of data should be communicated to them as well. In practice, this means if a user deletes their WordPress account, all other related accounts should also be deleted.

Users also have the [right to restriction](https://ico.org.uk/for-organisations/guide-to-the-general-data-protection-regulation-gdpr/individual-rights/right-to-restrict-processing/). This could occur if users request restriction of their data without deletion, or the company processing the data needs to restrict it while assessing a privacy claim.

Systems may need to be designed to allow restriction, such as marking a user restricted. This should prevent data processing while retaining the information. This could also take the form of an export; for example, removing a user entry from a database table completely while it is restricted, provided the data can be restored if the restriction is later removed.

## Other Jurisdictions

### Other European Laws

While GDPR covers the baseline laws for Europe and Europeans, specific jurisdictions may have further laws that require compliance. It is typically the client’s responsibility to comply with these, and requirements from them should be communicated to us and implemented. If you work in Europe, you should also be aware of your responsibilities when producing software for clients.

### United States

The US does not have a uniform privacy regulation, however some of the laws may apply when building software for US clients. Additionally, state-based laws may be stricter than country-wide laws. Generally speaking, following the GDPR guidelines should also comply with US guidelines.

### Australia

The Australian [Privacy Act](https://www.oaic.gov.au/privacy-law/) only applies to specific organisations and uses of personal information. This generally does not apply to HM, but may apply to our clients. Generally speaking, following the GDPR guidelines [should also comply with Australian guidelines](https://www.oaic.gov.au/engage-with-us/consultations/australian-businesses-and-the-eu-general-data-protection-regulation/consultation-draft-australian-businesses-and-the-eu-general-data-protection-regulation).
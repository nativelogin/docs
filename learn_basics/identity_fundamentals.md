# Identity fundamentals

## Introduction to Identity Management (IM)

Identity provides control over user validation. 

Commonly known as IM, it is different to IAM - identity and access management system. 

IAM implement a user validation and resource access: authentication and authorization.

We believe these two parts should be split for a reason.

## IM basic concepts

There are a technical terms, which are using in in IM industry.

- a *digital resource* is a application and data in a system. It could be web app, backend api, platforms, databases.
- *identity* - something or someone who wants to access to a *digital resource*. It could be a user, employee, admin. In IM, a user account is a *digital identity*. User account could be non-human entity: service in micro-service architecture, other system, IoT device, some hardware.

- *authentication* or simply *auth* is a validation of a *digital identity*. Someone, a trusted entity, should authenticate the user is who they claim to be. 
- *authorization* or simply *authz* when some trusted entity determining what resource and when some user or system can access.

## The difference between authentication and authorization.

Authentication and authorization are two important concepts in computer security. 

Authentication is the process of verifying a user's identity, which typically involves providing a username and password to access resources or applications. Authorization, on the other hand, is the process of determining which specific actions a user is allowed to perform within those resources or applications. Authentication confirms that the user is who they say they are, while authorization grants the user access to certain resources or applications and also defines what actions they are allowed to do with those resources.

Authentication can be done by various methods such as passwords, biometric scans, tokens, digital certificates and single sign-on systems.

If you ever traveled across the world, you have already faced auth and authz by yourself. 

As an Australian citizen, journeying to Ukraine requires your passport and visa. When you reach the Ukrainian border, a customs officer will inspect your documentation beginning with validating who you are through inspection of essential details including: name, photo, date of birth, gender and expiration date found on your passport.

Firstly, the person in charge will verify that your passport is real and not counterfeit. The office only recognizes documents issued by the Australian government, which is a Identity Management (IM) system for your passport.

To prevent any fraudulent activity, every Australian passport is designed with security measures that can only be issued by the Australian government.

If you were to obtain a fake passport from a joke store or print one out using your high-quality printer, the office would not accept it as it has not been issued by a trusted IM. Sorry, but other governments dows not trust you or joke stores  🤷🏻‍♀️ :-)

The act of verifying that the passport belongs to you and that all the details mentioned in it such as your name, age, etc. are accurate is called authentication.

Australian citizens cannot enter Ukraine without obtaining a visa from the Ukrainian embassy in Canberra. The visa issued by the embassy has a set expiration date and a specified number of entries permitted.

The visa granted to Australian citizens will depend on the purpose of their visit. For instance, a tourist visa permits individuals to engage in tourist activities only while a work visa allows them to work within the country.

If an Australian citizen does not have a valid visa, they will not be permitted to enter Ukraine by the immigration officer. 

This process is called authorization.

It is evident that the Identity Management System and Authorization System are separate entities that serve different purposes.

Our belief is that the Authorization system is a component of the service, distinct from the Identity system. For this reason, the Authorization system is not included in NativeLogin.

To explain this further, consider the example of the Ukrainian visa. This visa is only applicable in Ukraine as Ukraine is the resource that the user is attempting to access. In order to validate and authorize the access, Ukraine has its own gatekeeper, which is the border control.

It is important to note that having a Ukrainian visa does not grant access to other countries, such as the US, as each country has its own border control with different requirements for Australian citizens.

Every country (service) manages their own boarder control (authorization system).

## What does IAM do

Identity management allows you to manage user validation by determining:

- How users can join your system
- What user information to collect and store
- How users can demonstrate their identity
- When and how often users need to verify their identity
- The user experience during the identity verification process.

A simple IM solution is unlikely to meet the needs of users, organizations, industries, and compliance standards. 

In reality, IM is complex and typically requires a combination of various capabilities such as:
- seamless signup and login experiences that reflect your brand
- the ability for users to log in using multiple sources of user identities, such as social media and enterprise identity providers like Microsoft Active Directory
- multi-factor authentication (MFA) to provide additional proof of identity beyond just passwords
- step-up authentication for access to advanced capabilities and sensitive information 
- attack protection to prevent unauthorized access by bots and bad actors.

Developing such a system from scratch demands considerable time and resources. Given the level of complexity, many developers opt for an IM systems like NativeLogin instead of building their own solutions.


## How does IAM work? 

"IAM" or "Identity and Access Management" is not a singular, clearly defined system, but rather a discipline and framework for addressing the issue of secure access to digital resources. There are numerous approaches to implementing an IAM system and this section examines the common elements and practices utilized in such implementations.

### Identity providers (IdP)

In the past, the standard approach for identity and access management was for each system to generate and manage its own user identity information. This meant that users had to fill out registration forms and create a new account each time they wanted to use a new web application. The application stored all their personal information, including login credentials, and performed its own authentication when the user signed in.

As the internet expanded and more applications became available, users started accumulating numerous user accounts, each with its own login credentials. Although many applications still operate in this way, others have shifted to using identity providers to simplify their development and maintenance responsibilities, as well as to reduce their users' effort.

An identity provider is responsible for creating, managing, and maintaining identity information, as well as providing authentication services to other applications. Google Accounts is an example of an identity provider that stores account information such as your user name, full name, job title, and email address. Udemy, the online learning platform, enables users to log in with Google (or another identity provider) instead of going through the time-consuming process of entering and storing their personal information.

Identity providers do not disclose the user's authentication credentials with the applications that depend on them. For instance, when you use Google to sign in to Udemy, Udemy never gets access to your Google password. Google only confirms that you have verified your identity.

Aside from Google, other examples of identity providers are social media platforms like Facebook and Twitter, enterprise systems like Microsoft Active Directory, and legal identity providers such as [The Australian Government Digital Identity System](https://www.digitalidentity.gov.au/system-partners).

### Authentication factors

Authentication factors refer to techniques used to verify the identity of a user. There are various types of authentication factors, some of which are listed below:

| Factor | Description | Example |
| --- | --- | --- |
|  Knowledge | Something you know | Pin, password |
|  Possession | Something you have | Mobile phone, Email box, encryption key device |
|  Inherence | Something you are | 	Fingerprint, facial or voice recognition, iris scan |

Identity management (IM) systems offer various options for user authentication, ranging from anonymous login to multiple factors. The specific factors required can depend on the business needs and scenarios. Anonymous login, for instance, allows users to access a system without providing any authentication factors. On the other hand, some scenarios may require more secure access, and the system may need to implement multiple factors for user authentication. Ultimately, the decision of which factors to require depends on the level of security necessary for the system and the risks associated with the data or resources being accessed.

## Auth and authz standards


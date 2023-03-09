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

### OAuth2

OAuth, currently known as OAuth 2.0, is a publicly available framework that facilitates API authorization. It is essential to comprehend that OAuth is an authorization protocol, not an authentication protocol, although user data can be used for authorization purposes. It specifies the method by which an API client can procure security tokens that contain a set of authorizations regarding the resources available through that API.

Rather than necessitating that users share login credentials with one application to grant that application access to another, OAuth defers authorization decisions to a distinct authorization server that houses the user account. Essentially, OAuth serves on behalf of the user, enabling delegated access to a third-party service without the user revealing their credentials to that third party.

It is vital to note that OAuth is not an authentication protocol. For instance, OAuth does not prescribe a specific token format or a standard set of scopes for the access token. Moreover, it does not address how a secured resource authenticates an access token.

In addition, OAuth supports service-to-service and device-to-service scenarios, which SAML does not.

### OIDC

The OpenID Connect (OIDC) protocol is the newest security protocol that provides an authentication and identity layer to OAuth 2.0. Like OAuth, it delegates user authentication to the service provider that hosts the user account and authorizes third-party applications to access the user's account. However, OIDC also enables access to mobile applications, APIs, and browser-based applications.

Whenever users sign in to an application or service utilizing OIDC, they are redirected to their OpenID provider, where they authenticate and are subsequently redirected back to the application or service.

It is advisable to use OIDC in the following scenarios:

Developing a new application from the ground up, particularly if the application will be accessible from mobile devices.
Needing to define what actions a user can take within an application after they have been authenticated.
Utilizing JSON Web Tokens (JWTs) instead of XML.

### JWT

JWT stands for JSON Web Token, which is a compact, self-contained format for securely transmitting information between parties as a JSON object. It is used for authentication and authorization purposes in web applications and APIs.

A JWT consists of three parts: a header, a payload, and a signature. The header typically contains information about the type of token and the cryptographic algorithms used to secure it. The payload contains the information that needs to be transmitted, such as user ID, user role, or expiration date. The signature is generated using a secret key and is used to verify the authenticity of the token.

JWTs are commonly used for authentication by exchanging them between the client and the server. When a user logs in to a web application, the server generates a JWT and sends it to the client. The client then includes the JWT in subsequent requests to the server to access protected resources. The server can verify the JWT to ensure that the user is authenticated and authorized to access the requested resource.

### SSO

SSO stands for Single Sign-On, which is a mechanism that allows users to authenticate once and access multiple applications or services without having to enter their credentials again. It eliminates the need for users to remember different usernames and passwords for each application or service they use.

With SSO, a user logs in to a centralized authentication system, also known as an identity provider (IDP), which then authenticates the user's credentials. Once the user is authenticated, the IDP generates a token that can be used to access other applications or services that are part of the same SSO system.

There are several benefits of using SSO. It improves security by reducing the risk of weak passwords, phishing attacks, and password reuse. It also improves user experience by eliminating the need to remember multiple usernames and passwords, and reduces the administrative burden of managing multiple user accounts across different applications and services.

Overall, SSO is a convenient and secure mechanism that simplifies the authentication process for users and administrators alike, and is widely used in enterprise and cloud environments.

### FIDO

FIDO, which stands for Fast Identity Online, is an open authentication standard that aims to provide strong authentication and reduce the reliance on passwords. It is developed by the FIDO Alliance, a non-profit organization consisting of leading technology companies, financial institutions, and government agencies.

FIDO employs a public-key cryptography-based approach to authentication, where a unique cryptographic key pair is generated for each user and stored on their device. When a user wants to authenticate, the device generates a challenge that is sent to the server. The user then uses their device to sign the challenge with their private key, and the signed response is sent back to the server for verification.

FIDO provides two main types of authentication: FIDO U2F (Universal 2nd Factor) and FIDO2. FIDO U2F uses a hardware token, such as a USB or NFC device, to authenticate the user. FIDO2, on the other hand, can use either a hardware token or a biometric factor, such as a fingerprint or facial recognition, for authentication.

FIDO aims to improve security by eliminating the need for passwords, which are often weak and susceptible to attacks such as phishing and credential stuffing. It also provides a more user-friendly authentication experience by reducing the number of steps required for authentication and eliminating the need to remember and manage passwords.

Overall, FIDO is a promising technology that has the potential to improve authentication security and usability, and is being increasingly adopted by organizations and service providers.

### Passwordless login

Passwordless login is a method of authentication that eliminates the need for users to enter a password to access an application or service. Instead of relying on a password, passwordless authentication uses alternative factors, such as biometrics (e.g., fingerprint or facial recognition), security keys (e.g., USB or NFC devices), or one-time codes delivered via SMS or email, to verify the user's identity.

Passwordless login is gaining popularity because it addresses many of the security and usability issues associated with traditional password-based authentication. Passwords are often weak, easily guessed, or stolen, leading to data breaches and account takeover attacks. They also require users to remember and manage multiple passwords, which can be a source of frustration and lead to poor password hygiene.

By eliminating the need for passwords, passwordless authentication provides a more secure and user-friendly authentication experience. Users no longer have to worry about remembering passwords or typing them correctly, and the risk of password-related attacks is greatly reduced.

Passwordless login can be implemented in various ways, such as integrating with biometric sensors on mobile devices, using hardware security keys, or leveraging one-time codes delivered through SMS or email. Overall, passwordless authentication is a promising approach that can help organizations improve security and usability while reducing the burden on users.


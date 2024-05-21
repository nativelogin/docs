# Best practices for user account, authentication, and password management

Managing user accounts, authentication, and password security can be challenging. These aspects of software development are often overlooked and given lower priority by developers and product managers, resulting in a less than satisfactory user experience for some users in terms of data security and ease of use.

The following are a set of best practices that have been developed over the past 50 years as a result of painful experiences, financial losses, and disasters in the industry.

To avoid encountering problems, it's crucial to adhere to the best practices when developing your IM system. 

By implementing each of these points, NativeLogin ensures that your IM system is secure and user-friendly.

## Hash the passwords

My most important rule for account management is to safely store sensitive user information, including their password. You must treat this data as sacred and handle it appropriately.

Do not store plaintext passwords under any circumstances!!

Your service should instead store a cryptographically strong hash of the password that cannot be reversed—created with Argon2id, or Scrypt. 

When designing your system, it's important to consider the possibility of a security breach. Assume that your system will be compromised at some point and ask yourself how it could affect your users' safety and security on your service or other services they use. Additionally, think about ways to reduce the potential damage in case of a data leak.

It's crucial to ensure that user passwords are always stored securely. If there's any possibility that a user's password could be produced in plaintext at any time outside of immediately after it is provided by the user, this indicates a flaw in your implementation that needs to be addressed.



### The password should be salted 🧂

A salt is a randomly generated and unique string that is added to each password during the hashing process. Using a salt ensures that an attacker must crack each hash individually, using the corresponding salt, rather than cracking one hash and using it to compare against all stored hashes. This makes it much more difficult to crack large numbers of hashes, as the time required grows with the number of hashes.

Salting also protects against attackers using pre-computed hashes or database-based lookups, such as [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table). Additionally, salting ensures that two users with the same password will have different hashes, making it impossible to determine if two users have the same password without cracking the hashes.

Modern hashing algorithms like Argon2id, bcrypt, and PBKDF2 include automatic salting, eliminating the need for additional steps when using them.

### You have to add a pepper too 🌶️

In addition to salting, a [pepper](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#peppering) can provide an extra layer of protection against attackers who may have gained access to a database through means such as a [SQL injection](https://www.w3schools.com/sql/sql_injection.asp) vulnerability or backup copies. Peppering strategies involve encrypting or [HMAC](https://www.okta.com/au/identity-101/hmac)-ing password hashes using a symmetric encryption key, which acts as the pepper. This process does not impact the password hashing function.

Unlike salts, which are unique and randomly generated for each password, a pepper is shared between all stored passwords. Therefore, it should not be stored in the database and must be kept as a secret in secure storage such as AWS KMS or Hardware Security Modules (HSMs). Use [OWASP Secret management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html) as a reference.

As with any cryptographic key, a pepper rotation strategy should be considered to further enhance security.

### Strong hash algorithm 💪🏽

Using deprecated hashing technologies such as MD5 and SHA1, as well as reversible encryption, should be avoided when it comes to password storage. It is also important not to attempt to [invent a new hashing algorithm](https://www.schneier.com/blog/archives/2011/04/schneiers_law.html), as this can lead to potential security vulnerabilities.

There are several modern hashing algorithms available that have been specifically designed for securely storing passwords. These algorithms are intentionally slow, unlike older algorithms such as MD5 and SHA-1, which were designed to be fast. The level of slowness can be adjusted by changing the [work factor](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#work-factors), which should be done properly.

It is important for websites to be transparent about which password hashing algorithm they use. By utilizing a modern algorithm with proper configuration parameters, it should be safe to publicly state which algorithms are in use and to have them listed on sites such as [Pulse](https://pulse.michalspacek.cz/passwords/storages).

The best algorithms for today, according to [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html#password-hashing-algorithms) and [Google](https://cloud.google.com/blog/products/identity-security/account-authentication-and-password-management-best-practices) are:
- [Argon2](https://en.wikipedia.org/wiki/Argon2) - your first choice by default
- [scrypt](https://www.tarsnap.com/scrypt/scrypt.pdf) - best choice for legacy systems
- [bcrypt](https://en.wikipedia.org/wiki/Bcrypt) - the second choice if Argon2 is not available, or PBKDF2 is required to achieve FIPS-140 compliance.
- [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) - recommended by [NIST](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecretver) and has [FIPS-140](https://en.wikipedia.org/wiki/FIPS_140) validated implementations. So, it should be the preferred algorithm when these are required.

### avoid near-duplicates

If your system needs to identify similar passwords, such as when changing "VeryStrong" to "verysTr0ng", it's recommended to store the hashes of commonly used variations that you want to ban. These variations should have all letters converted to lowercase for normalization. This can be done during password creation or when users log in for existing accounts. Whenever a user creates a new password, generate the same type of variations and compare their hashes to the ones from previous passwords. The same level of hashing security as with the actual password should be used.


## Use trusted third-party identity providers

Consider allowing users to authenticate through third-party identity providers, such as Google, Facebook, or Twitter, if it is feasible for your system. This can provide a more convenient and secure user experience, as users can use their existing accounts to log in without having to create a new username and password for your service. Additionally, third-party identity providers often have robust security measures in place, which can help to enhance the security of your system. However, it is important to thoroughly vet and evaluate any third-party identity providers that you choose to integrate with your system to ensure that they meet your security and privacy requirements.


## User identity and user account is not the same

It's important to remember that your users are more than just their email address, phone number, or unique username. These authentication factors should be changeable without affecting the content or personally identifiable information (PII) in the user's account. Instead, your users are the sum of their unique, personalized data and experience within your service, which should be stored and managed in a well-designed user management system.

By keeping user accounts and credentials separate, it becomes much easier to implement third-party identity providers, allow users to change their usernames, and link multiple identities to a single user account. To achieve this, it may be useful to have an abstract internal global identifier for each user and associate their profile and authentication data with that ID, instead of storing everything in a single record. This approach reduces coupling and increases cohesion between different parts of a user's profile.


## One account with multiply identities

It's important to allow users to link multiple identities, such as email addresses and third-party authentication providers, to a single account. However, this can create a problem if a user creates a duplicate account unintentionally by using a new identity provider without realizing it.

To prevent this, it's important to separate user identity and authentication in the system. When a user tries to link a new identity to their account, the system should ask for a common identifying detail, such as an email address, phone number, or username. If the data matches an existing user in the system, the user should be required to authenticate with a known identity provider and link the new identity to their existing account.

By properly handling the linking of multiple identities, the system can avoid creating duplicate accounts and ensure that users have a seamless experience when using different authentication methods.

## Allow complex and long passwords

If you choose to impose limitations on password criteria, it should be for a valid reason such as technical limitations. For example, passwords may not be able to include line feeds and carriage returns due to [HTML specification](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/password#value) limitations. However, users should be allowed to use special characters such as Unicode letters, [emoji](https://en.wikipedia.org/wiki/Emoji#Unicode_blocks)🤐, or even [Klingon](https://en.wikipedia.org/wiki/Klingon_scripts) characters if they desire. It is important to perform Unicode normalization to ensure compatibility across different platforms.

[NIST guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html#appendix-astrength-of-memorized-secrets) for password complexity and length are available to help ensure password security. Using a strong cryptographic hash for password storage can help solve many security problems. Hashes will always produce a fixed-length output, so users should be able to use passwords of any length.

Limiting password length may still be a good idea to conserve memory usage. A limit of 100kb is reasonable. However, users attempting to use an extremely long password are likely following password best practices, such as using a password manager, which allows them to generate and remember complex passwords even on limited mobile device keyboards.


## Keep your username requirements user-friendly to provide a positive user experience.

While it's understandable for sites or services to implement certain restrictions for usernames, such as disallowing hidden characters and whitespace at the beginning and end of a username, going too far with requirements can lead to user frustration and discourage engagement.

For instance, overly strict requirements like blocking characters outside of 7-bit ASCII letters and numbers, or requiring a minimum length of eight characters, can be detrimental to the user experience. Such restrictions may offer development benefits, but at the cost of user satisfaction. Why not allow user have a Cyrillic letters and username like "Їжак"?

In some cases, it may be best to assign usernames rather than allow users to create their own. If this is the approach taken for your service, it's important to ensure that the assigned username is user-friendly and easy to recall and communicate. Alphanumeric-generated IDs should avoid visually ambiguous symbols like "O0" or "l1" and it's also recommended to perform a dictionary scan on any randomly generated string to ensure there are no unintended messages embedded in the username. These same guidelines apply to auto-generated passwords as well.

## Verify user's email and phone number

Asking for a user's contact information is a common practice in many services, but it's essential to validate that information as soon as possible to avoid potential issues down the line. Without proper validation, users may unknowingly make a typo in their contact information, leading to frustration and lost time when they're unable to access their account.

To prevent these issues, it's crucial to send a validation code or link to the provided email address or phone number as soon as possible. This can be accomplished through automated processes or manual intervention, but the key is to ensure that the contact information is accurate and belongs to the intended user.

If contact information is not validated, there is a risk that the account may become orphaned and unrecoverable without manual intervention. Even worse, the contact information may belong to someone else, potentially leading to a security breach and handing control of the account to a third party.

By validating contact information early in the process, you can help prevent these issues and ensure a smooth user experience. This not only benefits your users but also helps to maintain the integrity and security of your service.

## Don't make a username immutable

First of all, even if you let user's change their username, it is a good idea to not let others to use their old name. For more details [read this](https://www.computerworld.com/article/2838283/facebook-yahoo-prevent-use-of-recycled-email-addresses-to-hijack-accounts.html).

To accommodate users who wish to modify their usernames, a solution is to offer aliases and let users choose their primary alias. This feature can be augmented with additional business rules as necessary, such as restricting the number of username changes per year or limiting display and contact options to the primary username. For email address providers, it's important to never re-issue email addresses. However, old email addresses can be aliased to new ones. In fact, forward-thinking email address providers might enable users to use their own domain name and select any email address they desire.

## Let user close the account and delete the data automatically

Some services intentionally require users to contact their support team before closing their accounts, as this allows the team to address any potential issues and potentially retain the user. However, in an ideal world, all user-generated data should be deleted along with the account. This must be balanced against compliance requirements such as PCI and GDPR, as well as the impact on other users' experiences and system security.

One solution is to allow users to schedule their account for automatic deletion at a future date. In some cases, specific procedures must be followed in accordance with industry regulations, such as for medical or financial records.

## Access to resource should have a reasonable time limit

Whenever user is authenticated, it should not be trusted forever. There is a [NIST guidelines](https://pages.nist.gov/800-63-3/sp800-63b.html#sessionreauthn) about session management and authentication assurance levels.

Sensitive actions such as account deletion or money transfer may require additional security checks. It is advisable to request a password or send an OTP code to confirm these sensitive operations.

Sessions should have a time threshold, after which the user must provide a password, a second factor of authentication, or log out. It is essential to determine the length of time a user can be inactive before re-authenticating. Verification of user identity is necessary for all active sessions if a password reset is initiated. Authentication or a second factor is required if a user modifies significant aspects of their profile or performs sensitive operations. Re-authentication is necessary if the user's location changes significantly in a short time. It may be necessary to prevent users from logging in from multiple devices or locations simultaneously.

When a user session expires or requires re-authentication, prompt the user in real-time or provide a mechanism to save any unsaved activity since their last authentication. It can be frustrating for a user to spend a lot of time filling out a form, only to lose all their input and must log in again.

## Use MFA (Multi Factor Authentication)

MFA is also known as 2FA, 2-step verification, two-factor authentication.

2FA stands for "Two-Factor Authentication," which is a security measure used to protect online accounts from unauthorized access.

With 2FA, users must provide two forms of authentication to verify their identity when logging into an account. The first factor is usually a password, while the second factor is typically something the user possesses, such as a mobile device or a security key.


First of all, SMS codes are not secure!!! and have been [deprecated by NIST](https://www.nist.gov/blogs/cybersecurity-insights/questionsand-buzz-surrounding-draft-nist-special-publication-800-63-3). You have to have a good reason to keep this method. Unfortunately it is still very popular. 

There are many alternatives to SMS 2FA that are more secure and affordable. As you have to pay for every single SMS send to the user 😱.
- using TOTP with apps like Google Authenticator or Authy
- email verification code
- email "magic link"
-  confirmation on mobile apps like Google's Mail app or Adobe's "Account Access" app, 
- hardware security key
- FIDO

It's important to note that the security of your user accounts depends on the strength of the 2FA or account recovery method you choose to use.

## Make user IDs case-insensitive

Your users may not be concerned about or able to recall the exact case of their username. Therefore, usernames should be fully case-insensitive. Storing usernames and email addresses in all lowercase and converting any input to lowercase before comparison is a straightforward task. Be sure to specify a locale or use Unicode normalization during any transformations.

It's essential to keep in mind that the world utilizes not only Latin letters, but also various non-Latin scripts. Therefore, user IDs should be case-insensitive for all Unicode characters, not just Latin letters.

The proportion of smartphones used by users is constantly increasing. Most smartphones provide autocorrect and automatic capitalization for plain-text fields. Prohibiting this behavior at the UI level may not be desirable or entirely successful, and your service must be robust enough to handle email addresses or usernames that were accidentally auto-capitalized.

## Always check the most recent security best-practices and guidelines

The field of cybersecurity is expanding rapidly every day, encompassing both hackers and developers. It's crucial to ensure that all software is kept up-to-date by taking the following steps: updating tools, frameworks, and migrating from deprecated tools and frameworks, and discontinuing the use of insecure methods (such as SMS 2FA or SHA1). Additionally, it's important to read and implement the latest recommendations. To stay informed, you can rely on a list of trusted resources that should be followed, including:

 - [The National Institute of Standards and Technology (NIST)](https://www.nist.gov/): NIST provides cybersecurity frameworks, guidelines, and best practices for various sectors, including government, healthcare, and finance.
- [The Cybersecurity and Infrastructure Security Agency (CISA)](https://www.cisa.gov/): CISA is a government agency responsible for protecting the nation's critical infrastructure from cyber threats. They provide resources and guidance for both individuals and organizations on cybersecurity best practices.
- Your local regulator, for Australia it is [ACSC](https://www.cyber.gov.au/).
- [The Open Web Application Security Project (OWASP)](https://cheatsheetseries.owasp.org/index.html): OWASP is a nonprofit organization that provides information and tools for web application security. They offer resources for developers, organizations, and individuals to improve their web application security.


## Check the data breach database for your user

To enhance the security of your customers, there are several measures that can be taken:

- Implement password-manager friendly input fields to make it easier for customers to use a password manager.
- Use simple language when communicating with customers to ensure that they understand the importance of security measures.
- Enforce periodic password changes and encourage customers to check backup codes for added security. If the highest level of security is desired, consider the use of hardware keys.


It is important to regularly check users' passwords using breach databases such as ["Have I been pwned?"](https://haveibeenpwned.com). This is necessary because users often reuse the same password across multiple services, and even if all the above-mentioned measures are implemented, the passwords could still be exposed if a third-party service is breached and the passwords are stored in plain text at those 3rd party service because ignoring these security rules.

To prevent such incidents, it is recommended to block all passwords that have been leaked and to notify users to reset their passwords immediately when a data breach is identified.


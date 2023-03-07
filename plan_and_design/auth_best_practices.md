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










 The mains rules for hash algorithms are:
 - should be [salted](https://en.wikipedia.org/wiki/Salt_(cryptography)) for each user individually
 - do not user deprecate hash algorithms: MD5, SHA1
 The hash should be salted with a value unique to that specific login credential. Do not use deprecated hashing technologies such as MD5, SHA1 and under no circumstances should you use reversible encryption or try to invent your own hashing algorithm. Use a pepper that is not stored in the database to further protect the data in case of a breach. Consider the advantages of iteratively re-hashing the password multiple times.

Design your system assuming it will be compromised eventually. Ask yourself "If my database were exfiltrated today, would my users' safety and security be in peril on my service or other services they use?” As well as “What can we do to mitigate the potential for damage in the event of a leak?"

Another point: If you could possibly produce a user's password in plaintext at any time outside of immediately after them providing it to you, there's a problem with your implementation.

If your system requires detection of near-duplicate passwords, such as changing "Password" to "pAssword1", save the hashes of common variants you wish to ban with all letters normalized and converted to lowercase. This can be done when a password is created or upon successful login for pre-existing accounts. When the user creates a new password, generate the same type of variants and compare the hashes to those from the previous passwords. Use the same level of hashing security as with the actual password. 
# NativeLogin

## Intro

NativeLogin (formerly known as "Identifo") is a customer identity system. With this tool, you can quickly implement user authentication in your applications, taking just a few minutes to set up.

The main ideas of NativeLogin are:
- Uncompromising security (uses latest best practices)
- Ease and painlessness for the users (you should not loose your customers who struggle with secure identity system)
- Cloud-nativeness: created to live in managed Kubernetes solutions (GKE, EKS, AKS), or platform-specific container orchestration services like AWS ECS, with easy horizontal scaling and integrations with cloud-native infrastructure
- Effortless setup and integration: just a few minutes to setup and run

NativeLogin uses native authentication tools for every popular platform:
  - OIDC for web
    - JWK access, refresh and id tokens
    - JWKS introspection
    - `/userinfo` endpoint
    - OIDC scopes
    - Self-hosted web login pages
    - Platform-hosted customizable web login app
    - Web authentication API with Face ID and Touch ID for Apple devices
    - Login with Apple for web
  - iOS and macOS:
    - One-tap security upgrade
    - iCloud Private Relay
    - iCloud Keychain verification codes
    - Passkeys in iCloud Keychain
    - KeyChain integration
    - Associated domains
    - Deep linking
    - Device token management
    - Sign in with Apple
    - TVOS simple sign in
    - Integrates Private Access Tokens Challenge instead of captcha
  - Android:
    - Login with Apple for Android
    - Native Sign in with Google
    - Integration with Google platform information
    - Biometric login
    - Autofill features
    - Account Manager integration
    - Native OAuth2 service authentication flow
    - Custom Account type support

As you can see, NativeLogin is designed to utilize the native tools available on the platform.

## Sections

- Start building
  - Native iOS app
  - Native Android app
  - Mobile Flutter app
  - Web SPA app
  - Regular Web app
  - Backend
- Learn basics
  - Identity fundamentals
  - NativeLogin overview
  - JWT tokens
  - Compare with other tools
- Configure NativeLogin
  - Service settings
  - Deployment
  - Applications in NativeLogin
  - APIs
  - Dashboard
  - Integrations
    - Databases
    - Email
    - SMS
    - Storage
    - Auth Gateways
    - Strapi integration
    - Webflow integration
  - Plugin system
    - Storage
    - Hooks
    - Monitoring
    - Token payload
    - Authz with Open Policy Agent
  - Customization
    - Login screens
    - Email
    - SMS
    - Login flows with management API
- Plan and Design
  - Authentication flows
  - Architecture scenarios
    - SPA with backend and admin panel
    - Mobile apps, admin panel, web app and M2M integration
  - Best practices
  - Professional services
- IM system and security in depth
  - The most secure web authentication

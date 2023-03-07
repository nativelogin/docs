# NativeLogin

## Intro

Native login (previously known as "Identifo") is customer identity system. You can quickly add user authentication to your applications in minutes.

The main ideas of NativeLogin are:
- no security compromise (use the latest best practices)
- easy and painless for the users (you should not loose your customers who struggle with secure identity system)
- cloud-native - created to live in managed cloud like k8s or ecs with easy horizontal scale and integrations with cloud-native infrastructure
- effortless setup and integration: 5 minutes to setup and run
- NativeLogin uses native-platform authentications tools for every platform:
  - OIDC for web
    - JWK access, refresh and id tokens
    - JWKS introspection
    - userinfo endpoint
    - OIDC scopes
    - self-hosted web login pages
    - platform-hosted customizable web login app
    - web authentication api with face ID and touch ID for apple devices
    - login with Apple for web
  - iOS and maOS:
    - one tap security upgrade
    - iCloud Private relay
    - KeyChain integration
    - associated domains
    - deep linking
    - device token management
    - sign in with apple
    - iCloud Keychain verification codes
    - Passkeys in iCloud Keychain
    - TVOS simple sign in
    - integrates Private Access Tokens Challenge instead of captcha
  - Android devices
    -  login with Apple for Android
    - Native Sign in with Google
    - integration with Google platform information
    - Biometric login
    - Autofill features
    - Account Manager integration
    - Native OAuth2 service authentication flow
    - Custom Account type support


As you can see Native Login is design to use native platform tools.



## Sections

- Start Building
  - native iOS app
  - native Android app
  - mobile Flutter app
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
    - databases
    - email
    - sms
    - storage
  - Plugin system
    - storage
    - hooks
    - monitoring
    - tokens payload
    - authz with Open Policy Agent
  - Customization
    - login screens customization
    - custom login screens
    - email customization
    - sms customization
    - custom login flows with management API
- Plan and Design
  - Authentication flows
  - Architecture scenarios
    - SPA with backend and admin panel
    - mobile apps, admin panel, web app and M2M integration
  - Best practices
  - Professional services


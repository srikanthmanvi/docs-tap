# Setting up a Tanzu Application Platform GUI authentication provider

Tanzu Application Platform GUI extends the current [Backstage's authentication plug-in](https://backstage.io/docs/auth/) so that you can see a login page based on the authentication providers configured at install time. This feature is a work in progress, and at the moment it should support the following authentication providers out-of-the-box:

- [Auth0](https://backstage.io/docs/auth/auth0/provider)
- [Azure](https://backstage.io/docs/auth/microsoft/provider)
- [Bitbucket](https://backstage.io/docs/auth/bitbucket/provider)
- [GitHub](https://backstage.io/docs/auth/github/provider)
- [GitLab](https://backstage.io/docs/auth/gitlab/provider)
- [Google](https://backstage.io/docs/auth/google/provider)
- [Okta](https://backstage.io/docs/auth/okta/provider)
- [OneLogin](https://backstage.io/docs/auth/onelogin/provider)

Follow the [Backstage authentication docs](https://backstage.io/docs/auth/) to configure a supported authentication provider.

We also support a custom OpenID Connect (OIDC) provider shown here:

- Edit your tap-gui-values.yaml (or your custom configuration file) to include an OIDC authentication provider. Configure the OIDC provider with your OAuth App values. Example:

    ```
    auth:
      environment: development
      session:
        secret: custom session secret
      providers:
        oidc:
          development:
            metadataUrl: ${AUTH_OIDC_METADATA_URL}
            clientId: ${AUTH_OIDC_CLIENT_ID}
            clientSecret: ${AUTH_OIDC_CLIENT_SECRET}
            tokenSignedResponseAlg: ${AUTH_OIDC_TOKEN_SIGNED_RESPONSE_ALG} # default='RS256'
            scope: ${AUTH_OIDC_SCOPE} # default='openid profile email'
            prompt: auto # default=none (allowed values: auto, none, consent, login)
    ```

    `metadataUrl` is a JSON file with generic OIDC provider configuration. It contains `authorizationUrl` and `tokenUrl`. These values are read from the `metadataUrl` file by Tanzu Application Platform GUI, and so they do not need to be specified explicitly in your authentication configuration above.
    
    For more information, see [this example](https://github.com/backstage/backstage/blob/e4ab91cf571277c636e3e112cd82069cdd6fca1f/app-config.yaml#L333-L347).

## <a id='allow-guest-access'></a>Allow guest access

If you want to enable guest access along with other providers, you can do it by providing the following flag under your authentication configuration:

  ```
  auth:
    allowGuestAccess: true
  ```

## <a id='customize-login'></a>Customize the login page

You can change the card's title and/or description for a specific provider with the following configuration:

  ```
  auth:
    environment: development
    providers:
      ... # auth providers config
    loginPage:
      github:
        title: Github Login
        message: Enter with your GitHub account
  ```

For a provider to show in the login page, it has to be properly configured under the `auth.providers` section of your values file.

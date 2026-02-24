# rolling-demo-customized-sign-in-page

This repository contains the `rolling-demo-customized-sign-in-page` RHDH frontend plugin. It is a custom workaround for the RHDH AI Rolling Demo Environment, that:

- Displays a warning/info banner above the sign-in card (e.g. to inform first-time users about registration sync delays).
- Enforces authentication through a specific identity provider — currently hardcoded to **Keycloak** via the `oidc` provider.

> [!IMPORTANT]
> This plugin is **not supported by Red Hat Developer Hub (RHDH)** in any way. It is just a custom workaround built specifically for the RHDH AI Rolling Demo environment.

## Running locally

1. Navigate to the workspace directory:

   ```bash
   cd workspaces/rolling-demo-customized-sign-in-page
   ```

2. Install dependencies:

   ```bash
   yarn install
   ```

3. Start the development server:

   ```bash
   yarn dev
   ```

   This starts both the frontend (`app`) and backend in parallel. The app will be available at `http://localhost:3000`.

## Building a new version of the plugin

These steps produce a packaged, distributable version of the plugin ready for use in a dynamic plugin setup.

1. Make sure dependencies are installed (if you haven't already):

   ```bash
   yarn install
   ```

2. Navigate to the plugin directory:

   ```bash
   cd workspaces/rolling-demo-customized-sign-in-page/plugins/rolling-demo-customized-sign-in-page
   ```

3. Export the plugin:

   ```bash
   npx @red-hat-developer-hub/cli plugin export
   ```

4. Package and push the plugin image:

   ```bash
   npx @red-hat-developer-hub/cli plugin package --tag <image-registry>/<org>/<image>:<tag>
   ```

   Replace `<image-registry>/<org>/<image>:<tag>` with your target image reference, for example:

   ```bash
   npx @red-hat-developer-hub/cli plugin package --tag quay.io/my-org/rolling-demo-customized-sign-in-page:latest
   ```

## RHDH configuration

To load this plugin as a dynamic plugin in RHDH, add the following to your `dynamic-plugins` configuration:

```yaml
- package: oci://quay.io/redhat-ai-dev/rolling-demo-customized-sign-in-page:<tag>
  disabled: false
  pluginConfig:
    dynamicPlugins:
      frontend:
        red-hat-developer-hub.backstage-plugin-rolling-demo-customized-sign-in-page:
          signInPage:
            importName: RollingDemoCustomizedSignInPage
```

Update the `package` field with the image reference of the version you built and pushed.

## Contributing

Feel free to open a PR to the plugin in case you have any suggestions in mind.

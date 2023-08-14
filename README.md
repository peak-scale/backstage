# Backstage

Backstage is an open platform for building developer portals. Powered by a centralized software catalog, Backstage restores order to your microservices and infrastructure and enables your product teams to ship high-quality code quickly — without compromising autonomy.

This is a demo Backstage app.

## Docs

- <https://backstage.io/docs/overview/what-is-backstage>
- <https://github.com/backstage/backstage>
- <https://github.com/guymenahem/how-to-devops-tools/blob/main/backstage/README.md>

## Getting started

Check the official [Getting Started](https://backstage.io/docs/getting-started) guide.

Some tips:

- Install nvm (<https://github.com/nvm-sh/nvm#install--update-script>), and Node 18:

    ```sh
    nvm install 18
    nvm use 18
    ```

- Install yarn 1

    ```sh
    npm install --global yarn
    yarn set version 1.22.19
    yarn --version
    ```

## Local development

```sh
export AUTH_GITHUB_CLIENT_SECRET=...
yarn install
yarn dev
```

## Deployment to K8s

Check out the `deploy/gke` folder.

# Dokploy Deployment GitHub Action

This GitHub Action triggers a deployment on your Dokploy instance as part of a CI/CD pipeline.

---

## Inputs

### `api_token` (required)

The Dokploy authentication token.  
You can generate this under your Dokploy profile by clicking **Generate API Key**.

### `application_id` (required)

The ID of your application in Dokploy.  
You can find this in the URL of your application.  
Example:  
If your application is located at `https://dokploy.example.com/dashboard/project/.../services/application/hdoihUG0FmYC8GdoFEc`,  
then your application ID is `hdoihUG0FmYC8GdoFEc`.

### `dokploy_url` (required)

The base URL of your Dokploy instance, without a trailing slash.  
Example: `https://dokploy.example.com`

### `service_type`

**Optional** Type of Dokploy service (`compose` or `application`)

**Default** `application`

### `title`

**Optional** Deployment title shown in the Dokploy deployments tab.  
When left empty, Dokploy uses its default title (`Manual deployment`).

### `description`

**Optional** Deployment description shown below the title in the Dokploy deployments tab.  
When left empty, no description is set.

## Requirements

This action builds its request body with [`jq`](https://jqlang.github.io/jq/), which is preinstalled on GitHub-hosted runners. On self-hosted runners, make sure `jq` is available on the `PATH`.

## Usage

Include this action in your GitHub workflow file like this:

```yaml
name: Dokploy Deployment Workflow

on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Dokploy Deployment
      uses: benbristow/dokploy-deploy-action@0.2.2
      with:
        api_token: ${{ secrets.DOKPLOY_AUTH_TOKEN }}
        application_id: ${{ secrets.DOKPLOY_APPLICATION_ID }}
        dokploy_url: ${{ secrets.DOKPLOY_URL }}
        service_type: application
        title: "Deploy ${{ github.sha }}"
        description: "Triggered by ${{ github.actor }} via GitHub Actions"
```
Note: If you encounter persistent 403 errors, it might be related to Cloudflare bot detection blocking the requests, so it’s worth checking on that separately.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any bugs or feature requests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

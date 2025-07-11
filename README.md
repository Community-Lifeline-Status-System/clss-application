# Community Lifeline Status System (CLSS) Deployment

This repository contains the source code and automated deployment scripts for the Community Lifeline Status System (CLSS) application.


## Project Structure

The key components of this repository are:

* **`.github/workflows/`**: This directory contains the GitHub Actions workflow files. Each file defines a deployment process for a specific hosting platform (e.g., `azure.yml` for Azure).
* **`clss_deployment.zip`**: A required zip file containing all the necessary files to run the deployment. This includes the core `deploy.js` script, the front-end application source code (in an `app` folder), and a configuration file (`config.json`).

## Core Deployment Script (`deploy.js`)

The central piece of automation is the `deploy.js` script, located within `clss_deployment.zip`. Regardless of the final hosting destination, this script connects to your ArcGIS organization and performs the following critical setup tasks:

* **Creates User Groups**: Sets up necessary user groups (e.g., `CLSS Admins`, `CLSS Users`).
* **Provisions Feature Service**: Creates or updates the core Feature Service that stores the application's data.
* **Creates Web Map**: Generates the Web Map used by the front-end application.
* **Registers OAuth Application**: Registers the application within ArcGIS to get a Client ID, enabling secure user authentication.
* **Sets Permissions**: Shares the created ArcGIS items with the appropriate user groups.

This script is designed to be called by any GitHub Actions workflow after the zip file is extracted.

## Configuration

The deployment process is configured entirely through GitHub Actions Secrets and Variables. You can set these up in your GitHub repository under **Settings > Secrets and variables > Actions**.

### Secrets (for sensitive data)

Create these as **Secrets**. They are encrypted and will not be exposed in logs.

#### Core ArcGIS Secrets (Required for all deployments)

| Secret Name     | Description                                                                                         |
| --------------- | --------------------------------------------------------------------------------------------------- |
| `ARCGIS_USERNAME` | The username of an ArcGIS user with administrative privileges to create items, groups, and users.   |
| `ARCGIS_PASSWORD` | The password for the ArcGIS administrative user.                                                    |

#### Deployment Target Secrets

##### Deployment to Azure Static Web App

| Secret Name                                  | Description                                                                                        |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `AZURE_STATIC_WEB_APP_DEPLOYMENT_TOKEN` | The deployment token for your specific Azure Static Web App. You can get this from the Azure portal. |

*(Note: The `GITHUB_TOKEN` is a built-in secret provided by GitHub Actions and does not need to be created manually.)*

### Variables (for non-sensitive data)

Create these as **Variables** in an environment called **CLSS Prod**. They are stored as plain text and are safe to be viewed in logs.

#### Core ArcGIS Variables (Required for all deployments)

| Variable Name     | Description                                                              |
| ----------------- | ------------------------------------------------------------------------ |
| `ARCGIS_PORTAL_URL` | The URL of your ArcGIS Online organization or ArcGIS Enterprise portal.  |
| `CLSS_DOMAIN_URL` | The domain/url that the CLSS site will be accessed from.  |
| `ARCGIS_OBJECT_SUFFIX` | A set of characters to include in the deployed ARCGIS artifacts names. For instance, if the suffix is entered as '_prod', the deployed feature service will be named CLSS_FeatureService_prod  |

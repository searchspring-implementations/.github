# Searchspring Managed Snap Repositories

https://github.com/searchspring-implementations

Repositories in this organization are supported by Searchspring and are configured to use Github actions to handle the testing, bundling and deploying of the Snap code.

## Directory Structure
Directory structures may vary from project to project, but should generally follow the structure below. 

```
project
 ┣ 📂 .github
 ┃ ┗ 📂 workflows
 ┃   ┗ 📄 deploy.yml
 ┣ 📁 public
 ┣ 📂 src
 ┃ ┣ 📁 components
 ┃ ┣ 📁 scripts
 ┃ ┣ 📁 styles
 ┃ ┗ 📄 index.js
 ┣ 📂 tests
 ┃ ┣ 📂 cypress
 ┃ ┃ ┗ 📂 integration
 ┃ ┃   ┗ 📄 results.spec.js
 ┃ ┗ 📄 cypress.json
 ┣ 📄 .gitignore
 ┣ 📄 package-lock.json
 ┗ 📄 package.json
```

## Node.js
Snap development is recommended to be done with Node.js v16 (Gallium LTS). The [Snap Github Action](https://github.com/searchspring/snap-action) Node.js runtime version is also v16.

It is recommended to install Node.js via [NVM](https://github.com/nvm-sh/nvm) to easily switch between various versions.

## Branching
The default branch for each repository is called `production`. The code in this branch is used to build the javascript bundle used on the live site. This branch has [branch protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches#require-status-checks-before-merging) which will restrict all commits to pull requests that have a passing `Snap Action` check. For this reason, any code changes must be made to a separate branch, tested, and then merged back into `production` via a pull request when ready to release. 

## Github Action
On commit, each branch runs a Github action (deploy.yml) that executes the following steps:
* install packages
* build snap bundle
* run e2e tests
* sync recommendations templates
* upload to CDN

Failure at any step in the process will halt the entire workflow - the bundle will not be uploaded to the CDN.

### Install Packages
Node packages are installed via the `npm ci` command. This command uses the `packages-lock.json` file for dependency resolution; it is important that this file be committed on all `package.json` changes.

### Build Snap Bundle
The build process utilizes [Webpack](https://webpack.js.org/) to create the Snap bundle file(s). The number of files will depend on the complexity of the codebase and the code splitting configuration.

### E2E Tests
End-to-end tests are run using [Cypress](https://www.cypress.io/). Each site should at a minimum run core tests of the build - additional tests can be added as needed. The tests are located in the `/tests/cypress/integration` directory.

### Upload to CDN
After all other steps have completed, the bundle build files (`/dist` directory) and public files (`/public` directory) are pushed up to the Searchspring CDN (content delivery network) are the paths are invalidated (to clear the edge caching).

The entire build process plus invalidation can take up to 5 minutes to process.

## Bundle Files
In the URL examples below, replace `[your_site_id]` with the valid six-digit alphanumeric site id for your site.

### Production Bundle
The production bundle should be used on live site implementations, it is uploaded to this URL:  
`https://snapui.searchspring.io/[your_site_id]/bundle.js`  

### Branch Bundles
All other branch bundles are uploaded to the CDN at this URL (replace `[branch_name]` with the name of the git branch):  
`https://snapui.searchspring.io/[your_site_id]/[branch_name]/bundle.js`  

## Integration
The bundle file is typically placed in a global header file on the site inside of the `<head>` tag. The sooner that the bundle file is loaded - the sooner that Searchspring results will render on the page.
```html
<script src="https://snapui.searchspring.io/[your_site_id]/bundle.js"></script>
```
The meta tag is necessary to allow for branch overrides (see below).

## Branch Overrides
To ensure that changes do not break a live site, branch protection rules are in place requiring changes to be made on a separate branch (not `production`) and then merged in via a pull request.

To preview a branch simply add the branch name as a `branch` parameter to the current URL. For example, if you are on `https://www.sellingthings.com` and you wanted to test the `fix-bug` branch, you would navigate to `https://www.sellingthings.com?branch=fix-bug` after which you would see the branch preview popup showing the details of the preview.

![image](https://user-images.githubusercontent.com/5060400/148846755-a291c5c5-d7a5-4ebb-8a25-0d1f465cd646.png)


## Step by Step Examples
Generic code change: https://github.com/searchspring-implementations/.github/blob/production/CHANGES.md

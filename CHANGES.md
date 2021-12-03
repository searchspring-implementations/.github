## Change Example
This example outlines the steps needed to make changes to a Snap project by working with the code on your local machine.

Using the terminal, navigate to the directory of your choosing and clone the repository to your computer (replace `[YOUR_PROJECT]` with the correct path):
```shell
git clone git@github.com:searchspring-implementations/[YOUR_PROJECT].git
```

If you already have a checkout of the repository ensure that the `production` branch is up to date before branching.

```shell
git checkout production
git pull
```

Create a new branch (replace `my-new-branch` with branch name):

```shell
git checkout -b my-new-branch
```

Install packages:
```shell
npm install
```

Run the development server and build watcher:
```shell
npm run dev
```

The dev server is set to run on port 3333. And the bundle should be served at this location: https://localhost:3333/dist/bundle.js

The dev server will also serve up all files inside of the `/public` directory. Sites may have mockup files here to test locally.

Make necessary changes to the project. Cypress tests should be updated/added as needed for stability.

Create new tests and/or verify that current tests are still passing (dev server must still be running):
```shell
npm run cypress
```

Or run the tests headlessly (dev server not required):
```shell
npm run test
```

Commit changes:
```shell
git add -A
git commit -m 'a nice commit message talking about what changes were made'
```

Push the new branch to Github (replace `my-new-branch` with branch name):
```shell
git push -u origin my-new-branch
```

Check the repository action status and verify that it completes successfully. https://github.com/searchspring-implementations

Optionally verify that the changes fixed the problem by using the version override. Navigate to the website where Snap is integrated:  
`http://yoursite.com/collection/things?branch=my-new-branch`

Once satisfied with the changes, the branch can be merged into `production`. This can be done via the command line, or via a Pull Request in Github.

```shell
git checkout production
git pull
git merge my-new-branch
git push
```

Merge conflicts may occur if the production branch has changed since the branch was created. Some conflicts must be resolved manually.

We recommend using a Pull Request inside of Github to merge branches. This extra step clearly identifies the incomming changes about to be merged so that any last minute issues can be resolved. Any merge conflicts are immediately identified and can even be resolved within Github. Pull requests also provide a method for requesting a peer review of your work before merging it in.

After the branch is merged into `production`, a Github action will kickoff, and if it succeeds, the changes will be published to the CDN and be live on the site.

Congratulations, you have safely fixed a bug in your Snap project.

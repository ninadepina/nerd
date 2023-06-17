# Frontend Development Setup Triple by Chanel Mepschen

The purpose of this documentation is to provide an overview of the recommended frontend development setup triple. This setup includes various tools and practices to enhance productivity, ensure code quality, and facilitate efficient deployment. The following sections outline the key components of the frontend development setup.

## Basics

### Git

Git is used for version control and collaboration within the development team. The following workflow is recommended:

1. Create a branch for the feature or bug fix being worked on.
2. Commit and push changes to the branch regularly.
3. Create a pull request to merge the branch into the main codebase.
4. Conduct code reviews and address feedback.
5. Once approved, merge the code into the main branch.
6. Deploy the changes to the live environment.

### NPM

NPM (Node Package Manager) is a package manager for JavaScript that allows the use of pre-existing libraries and packages. When considering which packages to use, consider the following factors:

1. Check the package's latest update to ensure it is actively maintained.
2. Review the number of weekly downloads; higher download counts generally indicate a reliable package.
3. Assess the number of contributors, as a larger community often leads to better support.
4. Consider the package size, especially if it has no dependent packages.
5. Verify the licenses associated with the package, avoiding those that are not suitable for production use.

## Setup

The frontend development setup triple involves choosing between vanilla JavaScript, libraries, or frameworks to expedite development and reduce repetitive code.

-   Consider transitioning to React or Next.js for improved search engine optimization (SEO) and efficient DOM loading.
-   Svelte is recommended for projects targeting smart TVs.

## Early Error Catching

To identify and resolve issues earlier in the development process, consider using TypeScript. TypeScript provides static typing, which can catch potential errors before runtime.

## Code Formatting & Fixing

To maintain consistent code style and automatically fix common issues, the following tools are recommended:

-   ESLint: A pluggable linter for JavaScript that identifies problematic patterns.
-   Prettier: An opinionated code formatter that enforces consistent code formatting rules.

## CSS

To reduce the likelihood of errors in CSS code, consider using Sass (Syntactically Awesome Style Sheets). Sass extends the capabilities of CSS, introducing features like variables, mixins, and nesting.

## Live Deployment

When deploying static websites or server-side rendered (SSR) applications, consider using Azure and Azure DevOps. The following steps outline the deployment process:

-   Set up Azure DevOps pipelines to automate the deployment process.
-   Configure pipelines to trigger on updates, ensuring efficient deployment.
-   Verify agent readiness and retrieve necessary variables.
-   Execute npm install, run validations and tests, build the application, and push changes.
-   Deploy to the desired environment, such as development (dev), acceptance (acc), or production (prod).
-   Implement code reviews as part of the deployment process.

## Preview Environments

To provide a preview environment for clients without exposing it to end-users, consider the following workflow:

-   Local development environment (local) for individual developers.
-   Remote environment for sharing progress and collaboration.
-   Main branch for integration and testing.
-   Development environment (dev) for broader testing and feedback.
-   Acceptance environment (acc) for client review and approval.
-   Production environment (prod) for the live application.

Note: It is important to maintain separate environments and follow the appropriate deployment workflow to ensure proper testing and client validation before deploying to the production environment.

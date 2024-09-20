# About the application

This application displays the people directory with its data provided via an external API hosted by [randomapi.com](https://randomapi.com/api/6de6abfedb24f889e0b5f675edc50deb?fmt=raw&sole&seed=123).

The application is written in React, and aims to highlight the use of Functional Components (Person, Name) for displaying the people directory.

Underneath the application, you will find a [framework](framework) that is responsible for rendering the main page at `http://localhost:3000`

Upon arriving at the web root /, there are two links on the page that will demonstrate both client side and server side rendering of the people directory page using cached data of the API call to fetch data.

* **/withSSRData**: data is fetched on the server, serialized to an in-memory cache and then transferred to the browser
* **/withoutSSRData**: data is fetched on the React frontend app, and cache is managed on the client side via a `use hook`

# Configuring this repository

I have made a few additions to the repository in order to make it more production-ready. Some of the sections below are suggested, and I have not implemented it due to time constraints; ideally to have these items completely set up and tested before calling it production.

## Code Linting

Added `eslint` library to perform code linting so that syntax warnings about using the language and the React framework could be effectively checked against, and provide recommendations on improving overall code quality as the application evolves.

Sensible defaults are applied to the `eslint.config.mjs` and possibly be adapted later on. In addition to the npm run script `lint`, I have added `lint:fix` as a convenience for contributors to fix common code warnings and issues.

Another area of exploration is we can install a plugin that performs static code analysis, scanning the codebase for potential security vulnerabilities and deviations from best coding practices. It can identify issues such as insecure coding patterns, improper handling of sensitive data, code smells, and violations of style guidelines. By analyzing the code before it runs, the plugin helps developers proactively address security risks and improve code quality, ensuring compliance with industry standards and best practices. E.g.: `SonarQube`, `Snyk`, `Dependabot`, `npm audit`.

## Unit Testing

Testing is integral to every piece of software written, no matter the size. I have added the Jest testing library to this repository. Tests would be required to pass in order to be production ready. In addition to `npm run test`, I have added `test:coverage` so that we can be better informed on the coverage.
We could potentially use the coverage report to determine whether it is ready for production deployment.

## E2E Testing

Though I have not added this aspect of testing in the repository, I do stress the importance of having these end-to-end tests written so we could increase our confidence of how the application would operate in a production environment — from the client browser's request to server and any external services that this application would rely in order to be successful. Playwright is a library that I have used in the past that supports a variety of browsers, and worth exploring further.

## Environment Variables

There are no `env` dot files in this project. Once this application is primed for testing and go-live, we'll need to have environment files for each environment we want to test or deploy to. As a starter, we only have one external API call for retrieving people data, therefore, we can potentially assign the url endpoint as an environment variable.

## Source Control and Continuous Integration

A `.gitignore` file could be added with files that we do not wish to commit to the repository. File patterns like `dist/`, `node_modules/` and other files can be added incrementally. Once the repository is set up and ready to be sourced control, it is also time to integrate with a CI server using some of the control checks we have established in our `package.json` run scripts. For example, `lint`, `lint:coverage`, `test`, along with a combination of build + test scripts that are essential control gates for reviewing/merging a pull request, and deployment. In GitHub Actions, we can establish this kind of norms using workflow file(s) to achieve our goal of continuous integration.

## Misc.: Serialized cache improvement

Like how it was implemented for the React use hook cache fetching, I believe we could improvise the serialized cache that is used during the server side rending to keep track of different API endpoints requested by the client. Currently, it is serializing based on the `url` that is passed.

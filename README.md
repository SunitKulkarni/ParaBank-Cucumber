# Playwright + Cucumber BDD Automation Framework

**Recommended project name:** **Playwright + Cucumber BDD Automation Framework**

A straightforward, professional name that tells recruiters and engineering teams what the repository demonstrates: browser automation with Playwright and behavior-driven test scenarios written in Cucumber/Gherkin. For a shorter repository name, consider `playwright-cucumber-bdd`.

## Project overview

This project demonstrates end-to-end browser testing with JavaScript, Playwright, and Cucumber. Its Gherkin feature file describes login scenarios for the public [ParaBank demo application](https://parabank.parasoft.com/parabank/index.htm), and Cucumber step definitions use Playwright to interact with the browser.

The repository also contains a separate Playwright Test example suite. Together, the examples show two popular approaches to browser automation: business-readable BDD scenarios and Playwright's built-in test runner.

## What it covers

- Gherkin feature scenarios for valid credentials, invalid credentials, a blank password, and a blank username.
- Browser interactions implemented with Playwright's Chromium browser.
- A successful-login check that verifies the Accounts Overview heading.
- Cucumber HTML and Allure report configuration.
- Jenkins pipeline configuration for running the Cucumber suite and publishing reports.
- GitHub Actions workflow configuration for running the Playwright Test suite.

## Technology stack

- JavaScript (CommonJS for Cucumber step definitions)
- [Playwright](https://playwright.dev/) for browser automation
- [Cucumber.js](https://github.com/cucumber/cucumber-js) and Gherkin for BDD scenarios
- Allure and Cucumber HTML reporting
- Jenkins and GitHub Actions for CI examples

## Repository structure

```text
.
├── .github/
│   └── workflows/
│       └── playwright.yml       # GitHub Actions: Playwright Test suite
├── features/
│   ├── login.feature            # ParaBank login scenarios
│   └── support/
│       └── login.js             # Cucumber steps and Playwright setup
├── notes/                       # Learning notes and BDD guide
├── tests/
│   └── example.spec.js          # Playwright Test examples
├── cucumber.js                  # Cucumber formatters and report output
├── Jenkinsfile                  # Jenkins pipeline for Cucumber
├── package.json
└── playwright.config.js         # Playwright Test configuration
```

## Prerequisites

- Node.js and npm
- Internet access to reach the ParaBank demo application
- Chromium installed for Playwright

Install the project dependencies and browser:

```bash
npm ci
npx playwright install chromium
```

On Linux CI agents, install the browser's operating-system dependencies as well:

```bash
npx playwright install --with-deps chromium
```

## Running the tests

### Cucumber BDD scenarios

```bash
npx cucumber-js
```

Cucumber reads its configuration from `cucumber.js`. The configured formatters write the HTML report to `reports/cucumber-report.html` and Allure results to `allure-results/`.

To generate an Allure report after a run:

```bash
npx allure generate allure-results -o allure-report --clean
```

The Cucumber steps currently launch Chromium in headed mode. On a headless CI agent, configure a virtual display or update the browser launch settings before running this suite.

### Playwright Test examples

```bash
npx playwright test
```

This runs the tests under `tests/` using the Chromium project configured in `playwright.config.js`. The HTML report is generated in `playwright-report/`.

## CI configuration

- **GitHub Actions:** `.github/workflows/playwright.yml` installs dependencies and browsers, then runs `npx playwright test` on pushes and pull requests targeting `main` or `master`. It uploads the Playwright report as a workflow artifact.
- **Jenkins:** `Jenkinsfile` runs `npx cucumber-js`, generates an Allure report, and publishes the Cucumber HTML report. The pipeline uses Windows `bat` steps and expects the relevant Jenkins report-publishing plugins to be available.

These pipelines currently exercise different suites: GitHub Actions runs the Playwright Test examples, while Jenkins runs the Cucumber scenarios.

## Current scope and next steps

This is a learning and demonstration project, not a production test framework yet. The negative-login scenarios currently log the page text instead of asserting specific error messages, and the repeated error-message step definition should be consolidated to avoid ambiguous step matching. Other useful next steps include moving test data into configuration, supporting headless execution, and adding assertions for the negative scenarios.

## Demo application

The scenarios target the publicly available ParaBank demo site. The valid-login example uses the demo credentials defined in the step definitions; do not use these credentials for any real account or service.

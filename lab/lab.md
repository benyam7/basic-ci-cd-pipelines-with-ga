### LAB | Building and Extending Your CI/CD Pipeline

#### Introduction

You’ve practiced setting up basic CI with GitHub Actions. Now, let’s push this concept further. Imagine you’re part of a team that needs reliable, fast feedback on code quality. In this lab, you’ll integrate CI steps that test and build your code, and then simulate a simple deployment step. The goal is to give you confidence that your code is always in a releasable state.

#### Requirements

-   Ensure you have a basic Java project ready.

-   Make sure you can run tests locally with mvn test or an equivalent command before pushing changes.

#### Submission

When complete, submit a link to your repository’s workflow runs or a pull request link on the Student Portal.

#### Instructions

-   Refer back to the lesson examples if unsure.

-   If you struggle, consider researching “GitHub Actions marketplace actions” for hints.

##### Tasks

###### Task 1: Implement a CI workflow

-   Create `.github/workflows/ci.yml` triggered by push and pull_request events on main.

-   Set up Java, run tests, and build artifacts if tests pass.

###### Task 2: Simulate a Deployment

-   Add a step that outputs a message or uploads the generated `.jar` file as an artifact.

-   Bonus: If you have time, try deploying to GitHub Pages or another environment, or tag successful builds.

###### Task 3: Validate

-   Make a small code change and push it.

-   Confirm tests run and the “deployment” step triggers only on successful builds.

-   Check the GitHub Actions tab and share any issues or successes with your peers.

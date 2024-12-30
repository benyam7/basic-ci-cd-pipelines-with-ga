### LAB | Building and Extending Your CI/CD Pipeline

#### Introduction

In today’s fast-paced software development world, delivering reliable features quickly is a top priority. Continuous Integration (CI) ensures that every new piece of code is automatically built and tested as soon as it’s pushed, preventing integration headaches and catching bugs early. Continuous Deployment (CD) extends CI by automating releases, meaning successful builds can be rapidly deployed to staging or production.

### Why is CI/CD So Important?

-   Fast Feedback Loop: Developers immediately see if their changes break anything.
-   Reduced Risk: Automatic testing and incremental deployments help prevent large-scale failures.
-   Consistent Quality: Code is always in a “ready-to-release” state, boosting confidence.
-   Collaboration: Everyone on the team works on the same main branch with frequent merges, reducing “integration hell.”

By the end of this lab, you’ll see how CI/CD fits into the broader software development lifecycle (SDLC)—from coding and testing to releasing and monitoring. This lab is split into structured, incremental tasks that will guide you from a simple test workflow all the way to a simulated deployment scenario.

### Lab Overview

We’ll break down the lab into three major parts:

1. Guided Exercise – Building your first CI workflow to run tests on every code push.
2. Semi-Guided Task – Extending the workflow with a deployment step and artifact management.
3. Challenge & Troubleshooting – A scenario where you’ll fix or enhance a failing pipeline, emulating real-world situations.

You’ll also see some interactive checkpoints and quizzes sprinkled throughout, which help reinforce learning.
Additionally, you can collaborate with peers or do pair programming to mimic real-world team scenarios.

Throughout this lab, you’ll see how changes flow from code commits to build and test, then optionally to a deployment.

#### Prerequisites

-   Java Project: A simple project with at least one test class (e.g., using JUnit).
-   Maven or Gradle: Ensure you can run your tests locally (`mvn test` or `./gradlew test`).
-   GitHub Account: So you can create and view GitHub Actions in your repository.

### Part 1: Guided Exercise – Your First CI Workflow

#### Objective

Create a workflow that triggers on every push/pull request to main, checks out your code, sets up Java, builds, and runs tests.

#### Checkpoints:

1. Before you begin, do you have at least one passing test in your local environment?
2. Is your GitHub repository ready with a `.gitignore` and basic `pom.xml` or `build.gradle`?

#### Steps

1. Create the Workflow File
    1. In your repo, add a file at `.github/workflows/ci.yml`.
    2. Paste the minimal workflow below, using step-by-step comments for clarity:

 <pre>
 name: Java CI # This is the label you'll see in the Actions tab

on: # Events that trigger this workflow
push:
branches: [ "main" ]
pull_request:
branches: [ "main" ]

jobs:
build_and_test:
runs-on: ubuntu-latest # The VM environment for the workflow

    steps:
      - name: Check out the repository  # Step 1: get the code from GitHub
        uses: actions/checkout@v3

      - name: Set up JDK 17             # Step 2: install Java 17 on the runner
        uses: actions/setup-java@v3
        with:
          java-version: '17'

      - name: Build and Test            # Step 3: run your build and tests
        run: |
          mvn clean install
          mvn test
          # or use: ./gradlew build && ./gradlew test
</pre>

3. Commit and push your changes.

4. Verify the Workflow
    - Go to your repository’s Actions tab and see if the workflow ran.
    - If green (passed), congrats! If red (failed), expand the logs to troubleshoot.

#### Quick Quiz

-   Why do we specify runs-on: ubuntu-latest?
-   What could happen if we forget to “checkout” the code before running tests?

(Answers can be found at the bottom of the document, or discuss with peers.)

### Part 2: Semi-Guided Task – Adding a Deployment Step

#### Objective

Extend your existing CI pipeline to simulate a deployment. This step will only run if tests pass, mimicking a simple Continuous Deployment flow.

#### Checkpoints:

1. Have you verified that the “Java CI” workflow passes consistently on push events?
2. Are your `.jar` files or any artifacts produced by your build?

#### Steps

1. Add an Artifact Upload

-   Edit your `.github/workflows/ci.yml` to upload the `.jar` file as a build artifact:
<pre>
-   name: Upload JAR artifact
if: success() # Only if previous steps were successful
uses: actions/upload-artifact@v3
with:
name: build-artifact
path: target/\*.jar
</pre>

2. Simulate Deployment - Below the upload step, add a “mock” or “real” deployment step:
 <pre>

-   name: Mock Deploy
if: success()
run: echo "Deployment simulated! Imagine this file is heading to production."
</pre>

-   (Optional) If you want to try a real deployment, store your credentials as GitHub Secrets and reference them here (e.g., AWS CLI).

3. Commit and Push

    - Trigger the workflow again, confirm that the new steps appear in the logs.
    - If everything passes, you should see both the artifact upload and the deployment simulation logs.

4. (Optional) Collaborative Element
    - Open a pull request and have a classmate review the changes.
    - Observe how the pipeline runs automatically on your PR.

### Part 3: Challenge & Troubleshooting Scenario

#### Objective

In this final portion, you’ll face a realistic scenario where the pipeline fails. Your task is to debug and fix it.

#### Situation:

You’ve just merged a pull request that accidentally broke a unit test. Now, every build fails. Additionally, a colleague tried adding a branch-specific deployment that won’t trigger properly. You need to fix both issues.

#### Steps

1. Fix the Broken Test

-   Check the Actions tab to see which test is failing.
-   Update your code or test to resolve the error, confirm it passes locally, then push.

2. Branch-Specific Deployment

-   Your colleague added: `if: github.ref == 'refs/heads/main'` to the deployment step, but it never seems to run—investigate why.

-   Hint: Double-check your triggers (`on:`) and branch references. Possibly use `pull_request` vs. `push` to `main`.

3. Reflect

-   What caused the pipeline failure?
-   How does CI/CD help you catch this type of issue quickly? 4.

#### (Optional) Advanced Challenge

-   Set up a notification step (Slack, email, or GitHub release notes) on successful deployments.
-   Add a simple quiz at the end for your teammates: “What are the benefits of branch-specific deployment logic?”

#### Troubleshooting Tips

1. No Tests Run

Check the logs to confirm the correct folder structure and commands (`mvn test` vs. `./gradlew test`).

-   Make sure your test dependencies (e.g., `JUnit`) are in `pom.xml` or `build.gradle`.

2.  Artifact Upload Fails

    -   Verify the file path: `target/_.jar` (Maven) vs. `build/libs/_.jar (Gradle)`.
    -   Ensure the JAR exists by the time you reach the upload step.

3.  Branch Condition Not Triggering
    -   Compare your workflow’s on: triggers with your if: github.ref condition. If you’re pushing to main, it should read `refs/heads/main`. For tags, `refs/tags/...`.
4.  Permission or Secret Errors
    -   Re-check your GitHub repository’s secrets in Settings > Secrets and variables > Actions.
    -   Make sure you reference them correctly in the YAML: ${{ secrets.YOUR_SECRET_NAME }}.

#### Quizzes & Answers:

1.  Why specify runs-on: ubuntu-latest?
    -   This determines the OS environment in which your steps execute. GitHub provides Windows, macOS, and different Linux OS versions.
2.  What if we forget to check out the code?
    -   The runner won’t have your source files, so `mvn test` or `./gradlew test` will fail because it can’t find the project.

Use these quiz questions to start discussions or encourage peer explanations.

#### Conclusion

By starting with a guided CI workflow, adding a semi-guided deployment step, and tackling a troubleshooting scenario, you’ve experienced the incremental complexity of CI/CD in a realistic setting. This lab structure closely mirrors how professional teams roll out CI/CD in stages—first ensuring tests pass consistently, then automating deployments, and finally handling real-world breakages.

-   You can now appreciate the significance of CI/CD in the software development lifecycle, how it fosters collaboration, and why it’s crucial for quality control.
-   Don’t forget to explore optional tasks like code coverage, notifications, and branch-specific deployment logic for more advanced scenarios.
-   Use your final pipeline runs to demonstrate your mastery of automated builds and deployments.

##### Happy building and deploying!

### Lesson Title: Basic CI/CD Pipeline with GitHub Actions

#### Lesson Overview ✏️

In modern software development, delivering reliable features quickly is essential. Imagine a team releasing new features daily to a global e-commerce platform—without breaking anything. Continuous Integration/Continuous Deployment (CI/CD) practices, powered by GitHub Actions, enable development teams to integrate changes frequently, run automated tests to catch bugs early, and seamlessly deploy successful updates to users.

In this lesson, you’ll start with a step-by-step guided example—we’ll build and configure a simple CI workflow in class, push code changes, see the tests run, and then optionally deploy. Along the way, you’ll learn how CI/CD reduces integration issues, speeds feedback loops, and keeps users happy. To provide a broader perspective, we’ll also compare GitHub Actions with other popular CI/CD tools, such as Jenkins and GitLab CI/CD.

#### Learning Objectives 📓

By the end of this lesson, you will be able to:

-   Identify common challenges teams face without a proper CI/CD system.
-   Explain the core principles and real-world benefits of CI/CD.
-   Configure and run a basic GitHub Actions workflow for a Java project.
-   Integrate automated testing into a CI workflow to ensure quality code.
-   Trigger builds and tests automatically upon code pushes or pull requests.
-   Extend CI workflows to include basic deployment steps (CD).
-   Understand and compare GitHub Actions with other popular CI/CD tools.

#### Why CI/CD? Common Challenges Without It

Before diving into the how, let’s look at what can go wrong without a CI/CD pipeline:

-   Long Integration Cycles: Developers often delay merging code because they fear breaking the main branch. Merging becomes risky and time-consuming.
-   Delayed Bug Detection: Without automated tests running regularly, bugs can linger for weeks or months before being discovered.
-   Inconsistent Environments: Manual builds and deployments often happen on different machines with slightly different settings, leading to “works on my machine” issues.
-   Slow Feedback: Developers might wait hours or days to learn whether their code is stable enough for production.
-   Manual Deployments: Publishing a release can involve tedious, error-prone manual steps—like copying files to servers or running scripts by hand.

CI/CD addresses these pain points by automating code integrations, builds, tests, and deployments—leading to rapid feedback and more stable releases.

#### Key Definitions and Examples 🔑

##### Concept 1: CI/CD (Continuous Integration/Continuous Deployment)

###### Definition:

CI/CD is a development practice where code changes are integrated and tested frequently (CI), and successful builds are automatically (or easily) deployed (CD). This ensures high-quality and rapid release cycles.

###### Real-World Example:

Picture a social media app that updates features daily. CI runs tests immediately after code pushes, catching issues before they reach users. If tests pass, a CD step might deploy to a staging site, and once approved, to production—often with no downtime.

#### Concept 2: GitHub Actions

###### Definition:

GitHub Actions is a platform integrated into GitHub that automates tasks like building, testing, and deploying code based on events (e.g., a push or pull request).

###### Example:

A common GitHub Actions workflow file (.github/workflows/ci.yml) can be configured to run Java tests every time code is pushed. If the tests pass, the workflow might proceed to build the project and optionally deploy a test version of the application to a cloud environment.

#### Visual Overview of GitHub Actions

Below is a high-level flowchart showing the typical process inside a GitHub Actions pipeline:

![Visual Overview of GithubActions](CI-CD-flowchart.svg 'Title')

    1.	Trigger: A push or pull request event kicks off the workflow.
    2.	Actions Runner: GitHub’s hosted runner checks out the code and sets up the environment.
    3.	Build & Test: The code is compiled and tests are run.
    4.	Deploy/Tag: If tests succeed, code can be deployed or artifacts can be tagged and published.

#### Step-by-Step Guided Example: Building Your First Workflow

##### Let’s walk through a simple CI workflow together:

#### Step 1: Create Your Java Project

-   Initialize a simple Java project with Gradle or Maven.
-   Add a basic Greeting class and corresponding JUnit test.

##### Example GreetingTest.java:

<pre>
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.\*;

class GreetingTest {

    @Test
    void shouldReturnGreeting() {
        String greeting = greet("World");
        assertEquals("Hello, World!", greeting);
    }

    private String greet(String name) {
        return "Hello, " + name + "!";
    }

}
</pre>

#### Step 2: Create a Workflow File

Inside your repository, create a new folder: `.github/workflows/` In that folder, add a file named `ci.yml` with the following contents:

<pre>
name: Java CI

on:
push:
branches: [ "main" ]
pull_request:
branches: [ "main" ]

jobs:
build:
runs-on: ubuntu-latest
steps: - name: Check out the repository
uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'

      - name: Build project
        run: ./gradlew build
        # Or: mvn clean install

      - name: Run tests
        run: ./gradlew test
        # Or: mvn test
</pre>

### Explanation:

-   `on::` The workflow triggers on `push` and `pull_request` events to the main branch.
-   `jobs::` Defines the tasks to run.
-   `runs-on::` Specifies the OS environment.
-   `Steps:`
    -   Checkout code
    -   Set up JDK 17
    -   Build and test the Java project

#### Step 3: Push and Observe

-   Commit your changes to the repository and push them to GitHub.
-   Navigate to the Actions tab in your GitHub repository to see your workflow in action.
-   If successful, you’ll see a green checkmark next to your latest commit.

#### Step 4: Optional Deployment

If your tests pass, you can add a deployment step, for example, uploading an artifact to AWS S3. In the same ci.yml, add:

<pre>
-   name: Deploy to Staging
    if: success()
    run: |
    aws s3 cp build/libs/myapp.jar s3://my-bucket/staging/
</pre>

Note: You’ll need to configure your AWS credentials as GitHub Actions secrets.

#### Handling Secrets & Sensitive Data

Often, you’ll need API tokens, database credentials, or deployment keys during your CI/CD process. Here’s how to handle them securely:

1. Use GitHub Actions Secrets: In your repository’s Settings → Secrets and variables → Actions to add sensitive data.
2. Refer to Secrets in the Workflow:
 <pre>

-   name: Configure AWS
run: |
aws configure set aws_access_key_id ${{ secrets.AWS_ACCESS_KEY_ID }}
aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET_ACCESS_KEY }}
</pre>

3.  Never hardcode credentials in workflows, code, or commit history.

#### Comparing GitHub Actions with Other CI/CD Tools

While GitHub Actions is highly integrated with GitHub, several other platforms exist:

1. Jenkins

    - Pros: Extremely flexible, large plugin ecosystem, can run on-premise.

    - Cons: Requires more configuration and maintenance of the Jenkins server.

2. GitLab CI/CD

    - Pros: Integrated seamlessly with GitLab repositories, free unlimited CI minutes for some plans, strong container registry integration.
    - Cons: Self-managed runners can be more complex to set up.

3. CircleCI / Travis CI / Azure Pipelines
    - Various degrees of pricing, infrastructure options, and ecosystem integrations.

#### Why GitHub Actions?

-   Built directly into GitHub, no separate server required.
-   Simple YAML-based configuration.
-   Wide range of community “actions” for tasks like linting, deployment, and container building.

#### Common Failure Points & Troubleshooting 1. Tests Not Running

-   Verify the workflow file name (must be in `.github/workflows/`) and the triggers (on: section).
-   Ensure your test commands are correct (Gradle or Maven).
-   Permissions or Authentication Issues
    -   Confirm that your GitHub Actions settings allow workflows to run.
    -   Check that secrets are correctly set up in your repository.
-   Misconfigured Environment Variables
    -   Use GitHub’s Actions tab to inspect logs.
    -   Add echo statements or print debug info to confirm environment variables are set.
-   Build Script Errors
    -   Make sure Gradle or Maven is properly configured.
    -   Check for typos in your build commands.

Additional Resources 📋

-   [GitHub Actions Documentation](https://docs.github.com/en/actions)
-   [CI/CD Concepts](https://codefresh.io/learn/ci-cd/7-ci-cd-concepts-you-must-know/)

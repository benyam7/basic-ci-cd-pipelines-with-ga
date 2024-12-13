### CFU: Basic CI/CD Pipeline with GitHub Actions

#### Introduction ✏️

This activity helps you practice what you learned. CI/CD is not just theory—let’s get hands-on. After finishing these exercises, try variations on your own or explore advanced steps online.

##### Concept 1 Exercise

###### Instructions:

1. Create or use a GitHub repository with a simple Java project.
2. Add a `.github/workflows/ci.yml` file that:
    - Triggers on push events to main.
    - Checks out the code.
    - Sets up Java 17.
    - Runs mvn test.

#### Prompt for Understanding:

-   [ ] Before showing the solution, ask yourself: What would happen if a test fails?

<details><summary>Click to view solution</summary>

<pre>
name: CI

on:
  push:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'
      
      - name: Run Tests
        run: mvn test
</pre>
</details>

##### Concept 2 Exercise

###### Instructions:

1. Extend your workflow: After tests pass, run mvn package.
2. Add a step that simulates deployment (e.g., print “Deployment simulated” or upload an artifact).
3. Push a code change and observe the workflow in the Actions tab.

<details><summary>Click to view solution</summary>

<pre>
name: CI

on:
  push:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: '17'
      
      - name: Run Tests
        run: mvn test

      - name: Build Artifact
        if: success()
        run: mvn package

      - name: Simulate Deployment
        if: success()
        run: echo "Deployment simulated!"
</pre>
</details>

#### Reflect:

-   [ ] How would you adapt this for a real deployment? For example, what if we uploaded the artifact to a cloud storage bucket?

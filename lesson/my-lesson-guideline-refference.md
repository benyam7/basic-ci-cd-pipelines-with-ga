### Session Flow (30-45 minutes total)

#### Introduction & Narrative Context (5 min):

-   Present a short story: “Our team releases features daily. Without CI/CD, manual testing and integration could cause delays and bugs slipping into production. With CI/CD, changes are integrated, tested, and deployed automatically.”

-   Ask students: “Can you recall a time when a small change broke something unexpected? How could CI/CD help?”

#### Demonstration (I DO) (15-20 min):

-   Show how to add a `.github/workflows/ci.yml` file to a Java project. Start with something simple (checkout + echo) and then add steps incrementally:

    -   Step 1: Checkout code
    -   Step 2: Set up Java
    -   Step 3: Run a simple test command

-   [ ] Highlight common pitfalls (e.g., missing `actions/checkout`, incorrect Java version).
-   [ ] As you build the workflow, periodically pause and ask prediction questions: “What step do we need next to ensure tests run properly?” This encourages participation.

#### Interactive Exploration (WE DO) (5-10 min):

-   Have students suggest improvements: “If we want to package the code after testing, what step should we add?”

-   Students can code-along if they have a repo ready, or they can watch and answer quick polls or chat questions.

-   Introduce a quick poll: "Which command runs the tests?" to keep them alert.
    -   A. mvn test
    -   B. mvn run
    -   C. javac test

#### Check For Understanding (CFU) Activity (10-15 min):

-   Before the CFU activity, do a quick summary: “We created a workflow that runs on push. It checks out code, sets up Java, and runs tests. Why is this valuable?”

-   Move to the activity where students create or modify their own workflow.

-   After a few minutes, have volunteers share their configuration or troubleshoot a common error.

# Debugging, Testing, Git, and Agile Interview Questions

## What this document covers

This document covers practical interview questions about debugging, testing, Git, code review, Agile, Scrum, QA collaboration, unclear requirements, and behavioral communication for full-stack roles.

## Interview Questions

1. **How do you debug a backend API issue?**

   I first reproduce the issue, check the request method, URL, headers, and body, then inspect server logs, error messages, database queries, and recent code changes.

   Example answer:

   ```text
   I start by reproducing the failing request with the same input. Then I check logs, confirm the API route is reached, inspect validation and database calls, and narrow the issue down step by step.
   ```

2. **How do you debug a frontend issue?**

   I reproduce the issue in the browser, check the Console for JavaScript errors, inspect the UI and CSS in Elements, and check API requests in the Network tab.

3. **What do you check when an API returns `500`?**

   A `500` means a server-side error. I check backend logs, stack traces, database connection, input data, environment variables, and recent deployments.

4. **What do you check when the frontend cannot call the backend?**

   I check the backend URL, server status, Network tab, CORS settings, request headers, authentication token, HTTP method, and whether the backend route exists.

5. **What is the browser Network tab?**

   The Network tab is a browser DevTools tool that shows HTTP requests and responses. It helps inspect status codes, request payloads, headers, and response bodies.

6. **What is logging?**

   Logging means recording useful information while the application runs. Logs help developers understand errors, request flow, performance, and unexpected behavior.

   ```javascript
   console.log("Creating user", { email });
   console.error("Failed to create user", error);
   ```

7. **What is unit testing?**

   Unit testing checks a small piece of code, usually one function or component, in isolation.

   ```javascript
   function add(a, b) {
     return a + b;
   }

   // Test idea: add(2, 3) should return 5
   ```

8. **What is integration testing?**

   Integration testing checks how multiple parts of the system work together, such as an API route, service, repository, and database.

9. **What is the difference between unit test and integration test?**

   A unit test checks one small part in isolation. An integration test checks several parts working together.

10. **What should be tested in a REST API?**

    A REST API should test successful responses, validation errors, authentication, authorization, not-found cases, database behavior, and correct status codes.

    ```text
    POST /api/users with valid body -> 201 Created
    POST /api/users with missing email -> 400 Bad Request
    GET /api/users/999 -> 404 Not Found
    ```

11. **What is mocking?**

    Mocking means replacing a real dependency with a fake version during tests. For example, a test may mock an API call, database repository, or email service.

12. **What is Git?**

    Git is a version control system. It tracks code changes and helps developers collaborate safely.

13. **What is a branch?**

    A branch is a separate line of development. Developers usually create branches for features, bug fixes, or experiments.

    ```bash
    git checkout -b feature/user-api
    ```

14. **What is a pull request?**

    A pull request is a request to merge code from one branch into another. It lets teammates review, discuss, and test changes before merging.

15. **What is code review?**

    Code review is when teammates inspect code changes for correctness, readability, security, maintainability, and test coverage.

    Example answer:

    ```text
    In code review, I check whether the change solves the requirement, is easy to understand, handles errors, includes useful tests, and does not introduce regressions.
    ```

16. **What is a merge conflict?**

    A merge conflict happens when Git cannot automatically combine changes from different branches. The developer must manually choose or combine the correct code.

17. **What is Agile?**

    Agile is an iterative way of building software. Teams deliver work in small increments, get feedback, and adjust as requirements change.

18. **What is Scrum?**

    Scrum is an Agile framework where teams work in short cycles called sprints. It includes roles and events like sprint planning, daily standup, review, and retrospective.

19. **What is sprint planning?**

    Sprint planning is a meeting where the team chooses the work for the next sprint and clarifies goals, priorities, and estimates.

20. **What is daily standup?**

    Daily standup is a short team meeting to share progress, plans, and blockers.

    Example blocker update:

    ```text
    Yesterday I finished the API validation. Today I am integrating it with the React form. I am blocked because the backend response format is different from the API contract, so I need clarification.
    ```

21. **What is retrospective?**

    A retrospective is a meeting after a sprint where the team discusses what went well, what did not go well, and what can be improved.

22. **What are acceptance criteria?**

    Acceptance criteria are clear conditions that define when a task is complete. They help developers, QA, and product owners agree on expected behavior.

    ```text
    Given a user submits an empty email
    When the form is submitted
    Then the user sees an email required validation message
    ```

23. **How do you collaborate with QA?**

    I share context about the change, clarify acceptance criteria, explain risky areas, help reproduce bugs, and provide test data or API examples when needed.

24. **How do you handle unclear requirements?**

    I ask clarifying questions, identify assumptions, confirm expected behavior with the product owner or team, and document the decision before implementing.

    Example answer:

    ```text
    If a requirement is unclear, I do not guess silently. I write down the options, ask for clarification, and confirm the expected behavior before building the feature.
    ```

25. **What are common behavioral questions for a full-stack role?**

    Common questions ask how you handle bugs, blockers, deadlines, code review feedback, unclear requirements, production issues, and communication with frontend, backend, QA, and product teammates.

## Practical Answer Examples

### Explaining a Bug Investigation

```text
I reproduced the issue first, then checked the browser Network tab and saw the API returned 500. I copied the failing request, checked backend logs, found a null value reaching the service layer, added validation, and covered the case with a test.
```

### Explaining Code Review

```text
I treat code review as a quality and collaboration step. I look for correctness, readability, edge cases, security concerns, and test coverage. I also try to explain suggestions clearly and ask questions when the intent is not obvious.
```

### Explaining Agile Workflow

```text
In an Agile workflow, the team breaks work into small stories, clarifies acceptance criteria, plans sprint work, builds and tests incrementally, reviews progress, and improves the process during retrospectives.
```

### Explaining How You Communicate Blockers

```text
When I am blocked, I explain what I tried, what is stopping progress, who or what I need, and the impact on the task. I raise it early so the team can adjust or help.
```

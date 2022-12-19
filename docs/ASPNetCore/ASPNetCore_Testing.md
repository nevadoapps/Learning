## Testing ASP.Net Core Application

Table of Contents:
- Overview of automated testing
- Test-Driven Development (TDD)

<details>
<summary>

## Overview of automated testing
</summary>
<p>

Testing is an integral part of the development process and automated testing becomes crucial in the long run. Here is a list of three broad categories that represent how we can divide automated testing:

- Unit tests: Unit tests focus on individual units, like testing the outcome of a method.

Unit tests should be fast and should not rely on any infrastructure such as a database. Those are the kind of test that you want the most because they run fast, and each should test a precise code path. 

- Integration tests: Integration tests focus on component interaction, such as what happens when a component queries the database or what happens when two components interact with each other. Integration tests often require some infrastructure to interact with, which makes them slower to run.

- Functional tests: Functional tests focus on application-wide behaviors, such as what happens when a user clicks on a specific button, navigates to a specific page, posts a form, or sends a PUT request to some web API endpoint. 

Functional tests focus on testing the whole application, from the user's perspective, from a functional point of view, not just part of the code, as unit and integration tests do.

Other test that we should be aware of as knowledge:
- Load testing
- Performance testing
- Regression testing
- Contract testing
- Penetration testing
- e2e testing

</p>
</summary>
</details>

<details>
<summary>

## Test-Driven Development
</summary>
<p>


</p>
</details>
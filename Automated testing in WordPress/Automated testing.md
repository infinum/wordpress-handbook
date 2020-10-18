### What is the point of testing in WordPress?

You may be wondering: what do I need tests for? I will see if my custom post type has been registered. Or if the change I made to my page template changes on the front end. And you are completly right.

In most cases, you won't need tests. Especially if working on just simple presentational websites. But more often we get requests from clients to implement some third party integration into our system. This requires more elaborate business logic.

Here is where tests shine through. Not only do they speed up your development process, they assure that you always code to the result you want. And in the future, if there is any change request, you can be sure that that change won't break your existing functionality.

Tests are a safety net when working on complex projects.

When working with some third-party API integrations, they are useful because you can just mock your requests. When coding you can just run a test to get the result from the API (mocked). Instead of actually calling the API.

### Levels of testing

There are different types of tests, and they all serve a different purpose. There is a concept called the test pyramid that groups software tests into departments based on speed of execution, distance from the code, development cost and reliability.

![Test pyramid](/img/test-pyramid-wp.png)

The basis of all the tests are unit tests. They are the easiest to perform. You test one piece of your code in isolation of other code. You depend on mocks. They are mostly used to validate that your methods and functions do what you want them to do. They are super fast (because you just run the code in isolation).

After that we have integration tests. We want to make sure that our code works within the context of our app. In our case it's the WordPress app. So we test if our modifications (mostly filters and actions) work the way we intended them to work. Or if the custom post types or REST routes are registered and work the way we want them to.

After that we have acceptance and functional tests. Acceptance and functional tests are very similar, with a distinction that acceptance tests are __testing the functionality of the project from the viewpoint of the business user__.
They are very similar to end to end (E2E) tests, but those are performed from the viewpoint of the QA engineer.

Acceptance and functional tests are reserved for UI testing. We want to check if our modifications work on the front end (if we have certain buttons, or if the form is working properly). They are easiest to write (given we have a good framework), but take most time to execute. You need to have an environment set up (usually Selenium) and then it needs to execute steps, so that takes time.

Last level is the manual testing by the QA, and UAT or user acceptance tests. QA engineers manually test every functionality, and users (usually clients) will also go through your app and test its functionality.

These are slowest as they are completly manual.

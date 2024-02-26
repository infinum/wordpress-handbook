> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
>
> -- <cite>Martin Fowler et. all, Refactoring / Improving the Design of Existing Code</cite>

There are several things to be mindful of when working on a project.

1. Always write secure code
2. Always write clean and testable code
3. Be your own QA engineer
4. Write tests (if the project demands it, or you have extra time)

Your code represents you. The way code is written tells a lot about a developer. Code should always be secure and follow the agreed-upon standards and practices.
This means you need to set up all the necessary tools that will help you automate code checks like PHPCodeSniffer, PHPStan etc.
Inputs should be sanitized and outputs escaped. Translation should also be escaped.

Never instantiate classes in another class. Use dependency injection instead (inversion of control).
When your class is longer than 500 loc, it's usually a sign that your class is doing too much, and that you are violating the single responsibility principle (SRP).

When you think you are done with a project, you probably aren't 😄.
Test your site on different screen sizes. Test your code on different devices, either by physically going to the 7th floor and getting them from the QA, or by using [BrowserStack](https://www.browserstack.com/).

Use the site from the perspective of end user. Try to notice if things like visible hover and focus states are missing. Write accessible code. Try to use keyboard on your site and see if it's possible to navigate through it.

Go through [pre-deployment checklist](/project-management/checklists).

### Releasing the feature to QA/UAT

Only when you've done all the requirements above should a QA engineer be assigned to do a thorough check.

The following is an excerpt from the book by Robert C Martin 'Clean Coder'

```md
QA Should Find Nothing

I’ve said this before, and I’ll say it again. Despite the fact that your company may have a separate
QA group to test the software, it should be the goal of the development group that QA find nothing
wrong.
Of course, it’s not likely that this goal will be constantly achieved. After all, when you have a
group of intelligent people bound and determined to find all the wrinkles and deficits in a product,
they are likely going to find some. Still, every time QA finds something the development team should
react in horror. They should ask themselves how it happened and take steps to prevent it in the future.

QA Is Part of the Team

The previous section might have made it seem that QA and Development are at odds with each
other, that their relationship is adversarial. This is not the intent. Rather, QA and Development should
be working together to ensure the quality of the system. The best role for the QA part of the team is to
act as specifiers and characterizers.

QA as Specifiers

It should be QA’s role to work with business to create the automated acceptance tests that become
the true specification and requirements document for the system. Iteration by iteration they gather the
requirements from business and translate them into tests that describe to developers how the system
should behave (See Chapter 7, “Acceptance Testing”). In general, the business writes the happy-path
tests, while QA writes the corner, boundary, and unhappy-path tests.

QA as Characterizers

The other role for QA is to use the discipline of exploratory testing to characterize the true
behavior of the running system and report that behavior back to development and business. In this role
QA is not interpreting the requirements. Rather, they are identifying the actual behaviors of the
system.
```

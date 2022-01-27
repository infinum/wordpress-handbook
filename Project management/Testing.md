Once your app (website) is ready for release you need to test it out. This part is different from the automated tests described in the [automated testing introduction chapter](/automated-testing-in-wordpress/introduction).

QA engineers usually call this part of the project smoke testing. You can also help them check if the site is functioning.

First, you need to identify the crucial functionality of the website.

This could be anything from displaying blog posts in a correct way, checking if the pagination works, or if some background task is working.

Anything the client considers 'mission-critical'.

You should identify these important features with the client before the project development phase.

Once you have those, notify the QA engineers of what they need to test. There are so many things to check these days in WordPress (especially the editor) that it makes no sense to test every single thing. Developers test most of the editor features, so we can assume they work. Unless they are some super custom editor additions.

This is the manual part of the testing and benefits from having a 'script' of some sort, or even better: user stories.

As described in [Atlassian's agile coach documentation](https://www.atlassian.com/agile/project-management/user-stories):

>  User stories are one of the core components of an agile program. > They help provide a user-focused framework for daily work — which drives collaboration, creativity, and a better product overall.

Having a user story gives clear instruction on what action needs to happen, and what outcome is expected.

For instance it can be:

_As a user, I can go to the home page, and see a list of latest 4 posts from the blog_

or

_As a content editor, I can create a new post, and add paragraphs, headings and images to the post._

You get the gist.

Having these predefined scenarios will help reduce bugs and ensure that the app functions as intended. Once you deploy a new feature the same script should be followed to ensure the app is working as intended (developers didn't introduce a regression).
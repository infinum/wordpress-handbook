All pull requests must go through at least one reviewer (two is better), and must get approval from all people who've reviewed it at any point.

The committer can designate reviewers themselves, but anyone is free to review any code and code owners should be consulted for parts of the codebase that have code owners.

A PR with requested changes must not be merged unless force-merging is approved by two project owners in writing (e.g. as a PR comment)

You should create a new PR review thread for any particular concern you have with the committed code. All threads should have a classification of concerns. See specific practices for particular concerns in the Concern classification section.

All conversations on pull request reviews must be resolved or otherwise addressed before the PR is merged.

Only you (the person creating the concern thread) can resolve the thread. In the case of conflicting opinions, a project owner can resolve a thread. If a project owner is the committer, they are not able to force-resolve threads on their own PRs and should ask another project owner to mitigate.

Given these rules, we need some guidelines on how to approach PRs.

## On PR review culture

From a certain standpoint, a PR review is a fight. This isn't necessarily a bad thing: in martial sports, sparring helps you and your opponent build up your skills and knowledge through practice. As the committer is expected to write the best code they can, the reviewer is expected to challenge the committer to do the best they can.

However, PR review isn't combat. Knocking someone out doesn't help your sparring partner become a better fighter, and nitpicking over variable naming, (not) using syntactical sugar or requesting changes that require hours of manual work doesn't make the committer a better developer.

The goal of the PR review "fight" is not to go to war with your colleagues. The ultimate goal of PR reviews is to prevent bad, smelly, vulnerably and buggy code entering the codebase, help us build a great, maintainable codebase and help you become a better engineer with every commit you make. Walking the fine line between helping your colleagues write better code and learn new skills, and nitpicking until you've picked on a thread is challenging sometimes. However, building great software is a team effort and requiring changes is a must when there are concerns to be addressed. Seek progress, not perfection.

In general, your instincts as an engineer will guide you towards what's a worthwhile fight and how you'll approach the review. For example, as a reviewer, you should never allow security concerns to be dismissed, you should leave a comment when someone uses `!important`, but they could have simply increased the specificity or used the Manifest and CSS variables, and so on. However, consider whether requesting changes to ask someone to refactor their whole block with an entirely different approach without agreement from project stakeholders, asking the committer to do hours of manual work or fix things unrelated to their work and similar are worthwhile from a business standpoint and how should you classify your concerns appropriately. This should not prevent you from raising concerns, but you should not block merging because of a variable name.

As the committer, you should be open and welcoming to comments on your PRs. Sometimes, you'll waste away twenty hours of work when working on a feature and it'll only get noticed on PR reviews. Sometimes, you'll be challenged about variable names. Sometimes, you'll absolutely know you're right about something and someone will write a suggestion that will make you want to gauge your eyes out. Sometimes, you'll be wrong - even when you think you're right. While these situations might be challenging to deal with emotionally, you should not feel like a PR review is a source of tension. Entirely the opposite: PR reviews should help you become a better engineer, and maintain and improve the quality and security of our codebase.

As a reviewer, and a committer, you should keep your cool and leave your ego at the door. Your comments should be appropriate (given your character), good spirited and well intentioned, reflecting our shared interest to build great products while maintaining quality, security and flexibility.

## Concern classification

When starting a PR review thread, you should consider how you would classify your concern.

* **nitpick**
	* a trivial concern that does not block merging
	* nitpicks can be dismissed trivially as well, but should be well-intentioned as feedback and well-received from the committer
	* example: documentation readme is poorly written
* **non-blocking**
	* a minor concern that does not block merging
	* **the committer must address non-blocking concerns as well, and leave a comment indicating whether and in which way they plan to address them in the future**
	* example: block example attributes are not properly set up, lint rules failing in unmodified files,
* **blocking**
	* a concern that you will block merging until it's addressed
	* examples: lint rules failing in modified files, indent slips, `// todo` or `console.log` left in the code, code style practices, non-logical properties used, etc
* **red flag**
	* a large concern that needs to be fixed before merging
	* examples: variable gets interpolated into SQL query without escaping etc.

You should not resolve threads until your comments are appropriately resolved, using opposing arguments (making you change your mind) or commits addressing your comments (appeasing your concerns).

You should indicate how you classify your concern. All threads are considered blocking concerns if not otherwise indicated.

## Requesting changes

You should request changes if you have one or more unresolved blocking or red flag threads. You should not block merging if you only have unresolved (but addressed) non-blocking concerns or nitpicks.

## Comment resolution process

When a comment is added to the PR, we shouldn't let it block the feature or bugfix from being released. Follow these rules about resolving the comments:

* Person who left the comment should resolve it
* Limit the discussion to 5-10 comments, if you can't find a common ground, schedule a meeting with a team to discuss it
* Scheduling a meeting should only be warranted if there are issues with the code that could influence security or something that could impact the performance of the app
* Leave a note on the type of the concern you are having
* In case of minor differences, code owners can resolve comments in order to keep the PR moving

## Merging

You can merge if:

* there are no reviewers requesting changing, **and**
* there are no unresolved conversations on the blocking or red flag level, following rules for resolving, **and**
* all non-blocking concerns have been addressed with indication of future plans for improvement, **and** all automated checks are successful
* **or**, with a comment indicating approval from another project owner, you are a project owner consciously making the decision to merge without addressing the requested changes, unresolved conversations and/or CI/CD checks

*Credit to: Mario Borna Mjertan for the document*

There are various stages to a project life cycle. The main stages and some details are outlined in the image below

![Project life cycle](/img/project-lifecycle.png)

The stages of the project life cycle are the following:

#### Start

This is the phase that developers, designers, PM and client agree on what the requirements of the project are.
Between project start and actual development can pass considerable amount of time. This is the project exploration phase.
Designers usually have many meetings and workshops with the client where they discuss the way the web/app should look like. In this phase developers focus on architecture of the project. Production environments and infrastructure (AWS, hosted solutions etc.). Functionality the site might need (plugins, etc.) and licenses.
Developers must also work with designers in crafting reusable components, making sure all the designs are doable in the given time frame. Designers can be creative, but that usually means longer development time.

#### Security

The security is the top priority that must always taken care of. It's a **continuous process**. The developer must always ensure his code is of top quality. Tools like linters, phpcs and phpstan help, but the final say has the code reviewer.
We must follow the recommended practices, as well as watching the [OWASP](https://www.owasp.org/index.php/Main_Page) list of security issues. The developer must also be mindful when using third party plugins, as they can have security issues. This is where site like [WPScan Vulnerability Database](https://wpvulndb.com/) can be useful.
Security must also be maintained on the server as well in the code. You should coordinate with DevOps on this matter (handling CORS, adding security headers to nginx etc.).
For more details check the [security chapter in the handbook](/project-management/security/security-in-wordpress).

#### Project setup

In this stage developers work with DevOps to set up the agreed upon architecture. For example:

- How will the staging server look like?
- Are we using docker?
- Will the project be hosted on ready hosted solutions (Kinsta, Pantheon etc.), or will it be on our AWS accounts?
- Are we going to use any AWS functionality (S3 buckets, KMS, Amazon Rekognition), or services like Redis?

This all depends on the complexity of the project of course. DevOps will help the developer set up a CI/CD pipeline (GitHub Actions). Team lead will set up a GitHub repository for you.

#### Development start

Sometimes we get legacy projects that we need to migrate to WordPress. In that case we first assess what the DB structure looks like. It's easier to migrate from WordPress to WordPress of course. If the data needs to be migrated from a non WP database, we usually use our [Eightshift Migrations plugin](https://github.com/infinum/eightshift-migrations) for this.

#### Project reviews

This, like security, is a continuous process. We have PR reviews and Lead Engineer reviews. Every couple of weeks a Lead Engineer will sit down with the lead developer on a project. They will look at the code, and discuss if some things could be improved. See if the project is going in the right direction. These reviews last for the duration of the project.

#### Development phase

This is a phase where new features are developed and deployed to staging environment for testing. Once a feature is developed, a pull request is made and another developer should check the code for any issues. Once deployed on staging QA and client can test the features which can then be improved on if needed.
Once all the testing is done, a deploy to preproduction is made where the client can add content and QA can perform smoke tests.

#### Final security check

After all agreed upon features for a single version are completed, a final security check has to be made. We can use tools like Sonarqube and WPScan to ensure everything is secured.

#### Production deploy (project handover)

Pushing to the main branch and tagging the version is the final step. After this DevOps can either set up a direct CI/CD pipeline to the production environment, or can do the deployment manually (depending on the project).
After that we should migrate the content from preproduction (source of truth) and the project is live and handed over to a client.

Depending on the client and company agreement, the project can be completely handed over to a client, or we can still maintain it and work on it.

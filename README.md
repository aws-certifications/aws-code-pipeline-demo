# aws-code-pipeline-demo

Officeal document: https://docs.aws.amazon.com/codepipeline/index.html.

Continuous integration is a software development practice where members of a team use a version control system and frequently integrate their work to the same location, such as a main branch.

Continuous integration focues on smaller commit and smaller code changes to integrate.

The basic challenges of implementing CI include more frequent commits to the common codebase, mainting a single source code repository, automating builds, and automating testing. Additional challenges include testing in similar environments to production, providing visibility of the process to the team, and allowing developers to easily obtain any version of the application.

Continuous delivery is a softwrae development methodlogy where the release process is automated. Each software change is automatically built, tested, and deployed to production. It expands on continuous integration by deploying all code changes to a testing environment, a production environment, or both after the build stage has been completed.

Continuous integration is focused on automatically building and testing code, as compared to continuous delivery, which automates the entire software release process up to production.

Continuous delivery is not continuous deployment, the point of continuous delivery is not to apply every chagne to production immediately, but to ensure that every change is ready to go to production.

Using continuous delivery, the decision to go live becomes a business decision, not a technical one. The technical validation happens on every commit.

CI/CD is key to delivering software features quickly, securely, reliably, and with zero tolerance for outages.

- Automate the software release process
- Improve developer productivity
- Improve code quality
- Deliver updates faster

Maturity and beyond

- More staging environments for specific performance, compliance, security, and user interface(UI) tests
- Unit tests of infrastructure and configuration code along with the application code
- Integration with other systems and processes such as code review, issue tracking, and event notification
- Integration with database schema migration (if applicable)
- Additional steps for auditing and business approval

Testing stages

- Setting up the source
- Setting up and running builds
- Building
- Staging
  - Integration testing
  - Component testing
  - System testing
  - Performance testing
  - Compliance testing
  - User acceptance testing
- Production

Summary of best practices

Do:
- Treat your infrastructure as code
  - Use version control for your infrastructure code
  - Make use of bug tracking/ticketing systems
  - Have peers review chagnes before applying them
  - Establish infrastructure code patterns/designs
  - Test infrastructure changes like code changes
- Put developers into integrated teams of no more than 12 self-sustaing members
- Have all developers commit to the main trunk frequently, with no long-running feature branches
- Consistently adopt a build system such as Maven across your organization and standardize builds
- Have developers build unit tests toward 100% coverage of the code base
- Ensure that unit test are up-to-date and not-neglected. Unit test failures should be fixed, not bypassed.
- Treat your continuous delivery configuration as code.
- Establish role-based security controls (that is, who can do what and when)
  - Monitor/track every resource possible
  - Alert on services, availbility, and response times
  - Capture, learn, and improve
  - Share access with everyone on the team
  - Plan metrics and monitoring into the lifecycle
- Keep and track standard metrics
  - Number of builds
  - Number of deployments
  - Average time for changes to reach production
  - Average time from first pipeline stage to each stage
  - Number of changes reaching produciton
  - Average build time
- Use multiple distinct pipelines for each branch and team


For more details: 

- https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/welcome.html.
- https://aws.amazon.com/devops/what-is-devops/

AWS CI/CD services: CodeStar, CodeCommit, CodePipeline, CodeBuild, CodeDeploy, CodeArtifact.
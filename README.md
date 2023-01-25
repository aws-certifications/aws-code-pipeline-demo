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

## [CodePipeline concepts](https://docs.aws.amazon.com/codepipeline/latest/userguide/concepts.html)

- Pipelines
  - Stages
  - Actions
- Pipeline executions
  - Stopped exections
  - Failed executions
  - Superseded executions 作废的
- Stage executions
- Action executions
- Transitions
- Artifacts
- Source revisions

Actions  
  Valid action types are source, build, test, deploy, approal and invoke.

Pipeline executions  
  A pipeline stage progress only one execution at a time.
  Pipeline executions traverse pipeline stages in order. 
  Valid statues for pipelines are InProgress, Stopping, Stopped, Succeeded, Superseded, and Failed.

Stage executions  
  A stage execution is the process of completing all of the actions within s stage.
  Valid statues for stages are InProgress, Stopping, Stopped, Succeeded and Failed.

Action executions  
  An action execution is the process of completing a configuerd action that operates on designated artifacts.
  Valid statused for actions are InProgress, Abandoned, Succeeded, or Failed.

Action types  
  Action types are preconfigured actions that are available for selection in CodePipeline.

Transitions  
  A transition is the point where a pipeline execution moves to the next stage in the pipeline.

Artifacts  
  Artifacts refers to the collectio of data, such as application source code, built applications, dependencies, definitions files, templates, and so on, that is worked on by pipeline actions. Artifacts are produced by some actions and consumed by others.

  Actions pass output to another action for further processing using the pipeline artifact bucket. CodePipeline copies artifacts to the artifacts store, when the action picks them up.

Source revisions  
  A source revision is he version of a source change that triggers a pipeline execution. An execution processes that source revision only. For Github and CodeCommit repositories, this is the commit. For S3 buckets or actions, this is the object version.

How pipeline executions are started  
  - Stop and wait
  - Stop and abandon
    When an action is abandoned in CodePipeline, the action provider continues any tasks related to the action.

We recommend that you use the stop and wait option to stop a pipeline execution. This option is safer because it avoids possible failed or out-of-sequence tasks in your pipeline.

You might want to use the stop and abandon option in the case where you have a custom action.

How executions are processed in a pipeline

Rule 1: Stages are locked when an execution is being processed.  
Rule 2: Subsequenct executions wait for the stage to be unlocked.  
Rule 3: Waiting executions are superseded by more recent executions.

Managing Pipeline Flow

- A transition, which controls the flow of executions into the stage. Transitions can be enabled or disabled. When disabled, pipeline executions cannot enter the stage, and these execution is called the inbound execution. After the transition is enabled, an inbound execution moves into the stage and locks it.

Similar to executions awaiting a locked stage, when a transition is disabled, the execution waiting to enter the stage can still be superseded by a new execution. When a disabled transition is re-enabled, the latest execution, enters the stage.

- An approval action, which prevents a pipeline from transitioning to the next action until permission is granted.

A stage with an approval action is locked until the approval action is approved or rejected or has time out. A time-out approval action is processed in the same way as a failed action.

- A failure, when an action in a stage does not complete successfully.

Recommended pipeline structure

It's best to group related actions within a stage so that, when the stage locks, the actions all process the same execution. For example, related test, deploy, and approval actions grouped together.

How Inbound Executions Work

An inbound execution is an execution that is waiting for un unavailable stage, transition, or action to become available before it moves forward. The next stage, transition, or action might be unvailable because:

- Another execution has already entered the next stage and locked it
- The transition to enter the next stage is disalbed.

Inbound executions operate with the following considerations:

- As soon as the action, transition, or locked stage becomes available, the in-progress inbound execution enters the stage and continues through the pipeline
- While the inbound executions is waiting, it can be manually stopped. An inbound execution can have an InProgress, Stopped, or Failed state.
- When an in inbound execution has been stopped or has failed, it cannot retried because there are no failed actions to retry. When an inbound execution has been stopped, and the transition is enalbed, the stopped inbound execution does not continue into the stage.

Input and output artifacts

Stages use input and output artifacts taht are stored in the Amazon S3 artifact bucket you chose when you created the pipeline. CodePipeline zips and transfers the files for input and output artifacts as appropriate for the action type in the stage.

When you use the console to create your first pipeline, CodePipeline creates an Amazon S3 bucket in the same AWS account and AWS Region to store items for all pipelines. Every time you use the console to create another pipeline in thtat Region, CodePipeline creates a folder for that pipeline in the bucket. It uses that folder to store artifacts for your pipeline as the automated release process runs. This bucket is named codepipeline-region-12345EXAMPLE, where region is the AWS Region in which you create the pipeline, and 12345EXAMPLE is a 12-digit random number that ensures the bucket name is unique.

Every output artifact in the pipeline must have a unique name. Every input artifact for an action must match the output artifact of an action earlier in the pipeline.

# Continuous Deployment Flow

The majority of the automated steps documented below will be handled by our CI setup detail in the [CI Best Practices](ci-best-practices.md).

## Planning

1. Discussion with product owner regarding requirements.
1. Discussion with tester regarding how to test the requirements.
1. For substantial changes schedule a [Technical planning meeting](technical-planning-meeting.md).
1. Metric decided for success of code change.
1. Timeframe for success decided.

## Code Change

1. The `staging` branch is checked out on the developer's machine.
1. The latest changes are pulled.
1. A new feature branch is created for the code change.
1. Work is tested locally during development.
1. Commits are created.
1. The new branch is pushed up to GitHub.
1. A new PR is created using the [PR template](pr-template.md).
1. Project's contributing document is double checked.
1. Automated tests and quality checks run on project.

## Review

1. Work is reviewed by another developer.
1. Work is reviewed by a senior or project guru.

## Silo Testing

1. Tester checks out feature branch on their machine.
1. Appropriate testing is performed locally.

## Staging (optional)

1. Tester merges feature branch into the `staging` branch.
1. Changes to staging are automatically deployed to staging environment.
1. Appropriate testing is performed alongside any other recent changes from the team.

## Go live

1. Tester merges staging branch into the `master` branch.
1. Changes to master are deployed automatically to production environment.
1. Minimal automated tests are performed to ensure a successful deployment.
1. Deployment notification sent out to team.

## Measure

1. Success of code change is measured by metric and timeframe.

# Feature flags

**Feature flag**, or **feature toggle**, or **release toggle**, or **feature flipper** is a software development concept that allows to enable or disable a feature without modifying the source code or requiring a redeploy.

Feature flags determine at runtime which portions of code are executed. This allows new features to be deployed without making them visible to users or, even more importantly, you can make them visible to only a specific subset of users.

At the most basic level, they take the form of simple conditionals that determine the code path that will be executed.

## How not to forget to turn on a feature flag

- Create a task in a backlog
- Set a deadline for the task
- In task description specify important information that is necessary for turning feature flag on
- Definition of done
  - Feature flag is turned on by default
  - Activation strategy is turned off

## When to delete code that's no longer used

- Create a task in a backlog
- Definition of done
  - Old code is removed
  - Activation strategy is done and removed
  - Feature flag is removed from administrative UI

## .NET client

[↑ Unleash FeatureToggle Client for .Net](https://docs.getunleash.io/reference/sdks/dotnetv).

## Links

- [↑ Feature Toggles (aka Feature Flags)](https://martinfowler.com/articles/feature-toggles.html)
- [↑ Tutorial: Use feature flags in an ASP.NET Core app](https://learn.microsoft.com/en-us/azure/azure-app-configuration/use-feature-flags-dotnet-core?tabs=core5x)
- [↑ Feature flags](https://docs.gitlab.com/ee/operations/feature_flags.html)
- [↑ Unleash, an open source feature management solution](https://github.com/Unleash/unleash)
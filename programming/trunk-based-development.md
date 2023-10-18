# Trunk-based development, TBD

The **trunk-based development** or **TBD** is a version control management practice where developers merge small, frequent updates to a core "trunk" or main branch.

TBD is a common practice among DevOps teams and part of the DevOps lifecycle since it streamlines merging and integration phases. In fact, trunk-based development is a required practice of CI/CD. Developers can create short-lived branches with a few small commits compared to other long-lived feature branching strategies. As codebase complexity and team size grow, trunk-based development helps keep production releases flowing.

## Gitflow vs trunk-based development

[↑ Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) is an alternative Git branching model that uses long-lived feature branches and multiple primary branches. Gitflow has more, longer-lived branches and larger commits than trunk-based development. Under this model, developers create a feature branch and delay merging it to the main trunk branch until the feature is complete. These long-lived feature branches require more collaboration to merge as they have a higher risk of deviating from the trunk branch and introducing conflicting updates.

Gitflow also has separate primary branch lines for development, hotfixes, features, and releases. There are different strategies for merging commits between these branches. Since there are more branches to juggle and manage, there is often more complexity that requires additional planning sessions and review from the team.

Trunk-based development is far more simplified since it focuses on the main branch as the source of fixes and releases. In trunk-based development the main branch is assumed to always be stable, without issues, and ready to deploy.

## Feature flags

Feature flags nicely complement trunk-based development by enabling developers to wrap new changes in an inactive code path and activate it at a later time. This allows developers to forgo creating a separate repository feature branch and instead commit new feature code directly to the main branch within a feature flag path.

## Links

[↑ Trunk-based development](https://www.atlassian.com/continuous-delivery/continuous-integration/trunk-based-development).

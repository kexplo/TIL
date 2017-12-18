## git flow, Finish hotfix branch when a release branch currently exists

http://nvie.com/posts/a-successful-git-branching-model/

The one exception to the rule here is that, **when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of** `develop`. Back-merging the bugfix into the release branch will eventually result in the bugfix being merged into develop too, when the release branch is finished. (If work in `develop` immediately requires this bugfix and cannot wait for the release branch to be finished, you may safely merge the bugfix into `develop` now already as well.)

https://github.com/nvie/gitflow/issues/177

SEE: https://github.com/petervanderdoes/gitflow-avh/issues/161

This may be optional. We got a release management including RC versions, which are based on the current release branch. However, currently we need to manually merge the master back into release branch when finishing a hotfix.
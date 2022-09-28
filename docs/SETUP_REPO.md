# Setting up github repository

Should be done once before starting any other project activity.

* create an empty github repository named "intellistart-java-2022-{your-team-name}"
* clone that empty repository locally
* clone as well the reference project `git clone git@github.com:gavluk-intellias/intellistart-java-project.git`
* copy from reference project all the template into your repo: `cp -R ./intellistart-java-project/* ../intellistart-java-2022-{your-team-name}`
* `git add . ; git push ....`
* make these steps to have working CI/CD (github "Actions") working:
  * create the [personal access token](https://github.com/settings/tokens) with permissions `repo.*` and `write:discussion` (expiration e.g. 90 days) 
  * create secret named exactly `REPO_COMMIT_TOKEN` and put the token you just created there: `Settings -> Security -> Secrets -> Action`
* make these steps to protect "main" branch from direct commits:
  * `Settings -> Cone and automation -> Branches -> Branch protection rules -> Add rule`:
    * Set `Branch name pattern` as `main`
    * Set `Require a pull request before merging`: true
    * Set `Require approvals`: 1
  * [manage](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-the-automatic-deletion-of-branches) the automatic deletion of branches: `Settings -> General -> Pull Requests -> Automatically delete head branches`
* invite all your team as collaborators: `Settings -> Access -> Collaborators`

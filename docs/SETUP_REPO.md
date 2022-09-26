# Setting up github repository

* create git repository named "intellistart-java-2022-<your-team-name>"
* clone this project `git clone git@github.com:gavluk/intellistart-java-project.git`
* `git clone` and your empty project 
* copy project template into your repo: `cp -R intellistart-java-project/* intellistart-java-2022-<your-team>`
* `git add . ; git push ....`
* create [personal access token](https://github.com/settings/tokens) with permissions `repo.*` and `write:discussion` (expiration e.g. 90 days) 
* create secret named exactly `REPO_COMMIT_TOKEN` and put the token you just created there: `Settings -> Security -> Secrets -> Action`
* 
* [manage](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/managing-the-automatic-deletion-of-branches) the automatic deletion of branches: `Settings -> General -> Pull Requests -> Automatically delete head branches`
* invite all your team as collaborators: `Settings -> Access -> Collaborators`

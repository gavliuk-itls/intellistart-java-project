# Setting up github repository

Should be done once before starting any other project activity.

You might just copy all the shell commands if you preliminary do export `TEAM` variable like `export TEAM=badboys`

## Setup repository

* create an empty github repository named "intellistart-java-2022-{your-team-name}"
* clone that empty repository locally `git clone git@github.com:{your-account}/intellistart-java-2022-${TEAM}`
* clone as well the reference project `git clone git@github.com:gavluk-intellias/intellistart-java-project.git`
* copy from reference project all the template files into your repo: `cp -r ./intellistart-java-project/intellistart-java-2022-dreamteam/. ./intellistart-java-2022-${TEAM}`
* check/set execution permissions for ./mvnw: 

```
git update-index --chmod=+x ./mwnw

# as well for linux or mac:
chmod 755 ./mwnw
```

```sh
cd ./intellistart-java-2022-${TEAM} 
git add . 
git commit -a -m "Initial commit" 
git push
```

* now you can go to `Actions` menu and see that the "Initial commit" job is in progress and then fail (only the "Add coverage PR" should be failed)
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


## Test it by proceeding the first pull request

* set up the Intellij IDEA codestyle: `IDEA: Settings -> Editor -> Code Style -> Scheme -> Import scheme...` and choose `intellij-code-style.xml` file
* make some branch for testing, like create an `README.md` file: 

```sh
git checkout -b test-change 
touch ./README.md 
git add ./README.md 
git commit -a -m "Test change" 
git push --set-upstream origin test-change
```

* go to the browser and create a pull request
* check the pull request has `All checks have passed` but for now has the `Review required` and `Merging is blocked`
* make any colleague as reviewer
* let this collegue do approve
* merge your first pull request!

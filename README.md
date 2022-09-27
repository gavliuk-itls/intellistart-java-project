# intellistart-java-project

## Overview

This is the test project for Intellistart program students.

The main goal is to demonstrate as much as possible all the aspects of working on real commercial project.

The general guide:

* [Start the team and the project](#start-the-team-and-the-project)
    * [Team Bootstrap meeting](#team-bootstrap-meeting)
    * [Bootstrap code session](#bootstrap-code-session)
    * [Grooming and planning](#grooming-and-planning)
* [Project proceeding](#project-proceeding)

Addons:

* [Setup github repository guide](./docs/SETUP_REPO.md)
* [Initial code session guide](./docs/INITIAL_DECOMPOSITION.md)
* [Functional requirements](./docs/REQUIREMENTS.md)
* [Non-functional requirement](./docs/REQUIREMENTS_NF.md)


## Start the team and the project

* Create your community space (telegram, discord, whatever)
* Organize first "Team Bootstrap" meeting (<1 hour)

### Team Bootstrap meeting

![Meeting1](./img/meeting1.png)

**Timing**: ~1 hour

**Why**: to meet each other and you need a repository, right?

* give the name to your team; one english/img/ word like "assasins", "neptun", "nirvana", etc.
* somebody shares the screen and do [repository setup](./docs/SETUP_REPO.md)
* everybody do `git clone` of your project and run `./mvnw clean package` to check everything is fine
* vote for 2 collegues who will do [initial decomposition](./docs/INITIAL_DECOMPOSITION.md)
* schedule two next meetings:
    * [Bootstrap code session](#bootstrap-code-session): just for 2 de-compositors :) 
    * [Grooming and Planning](#grooming-and-planning): for all

> **Notify menthor that meeting is passed, provide contacts of 2 decomposition volunteers and github repository URL**.

### Bootstrap code session

![Meeting 2](./img/meeting2.png)

**Timing**: ~2 hour

**Why**: a way to get a clue how to make decomposition and try pair programming

* read [Functional requirements](#functional-requirements)
* make some pair-programming session and create basic services and controllers, see [initial decomposition](./docs/INITIAL_DECOMPOSITION.md)
* suggestion: try to work on this session in Test Driven way: make some core functional tests at very high level (only positive scenarios, might be not completed or without assertions, etc) and make it working (green) by mocking and creating basic entity classes
* make pull request and initiate/schedule [Grooming and Planning](#grooming-and-planning)
* the commit subject would be `:rocket: Create essential stubs`

### Grooming and planning

![Meeting3](./img/meeting3.png)

Timing: ~2 hour

**Why**: to practice typical Grooming/Planning scrum meeting at the same time the first code review

> The hardest meeting. 
> 
> Even do not try to make an *ideal* or
> *complete* planning. The goal for now is just everybody 
> must have some clear things to do and assigned Issue.

In fact this will be the first Code Review session when all the team do code review of `Create essential stubs` pull request.

* 2 authors share the PR screen and present the code and explain the decomposition
* all the team discussing, if required
* somebody starts to create gitlab issues, use your estimate as suffix of subject, e.g.: `Implement InterviewerTimeSlotService: 8h`
* decide who will be assigned to each task: everybody must be assigned at least to one issue
* merge the reviewed pull request

At the end you got decomposition by services, so what you need is create issues in github:

* Implement service FooService
* Implement service BarService

Or maybe starting from scenarios (might be better way):

* Implement scenario Bla-bla
* Implement scenario Foo-bar

If you not sure what to do regarding some area, you might crearte like:

* Investigate Facebook oauth2 for AuthService
    * better to explain what result would be there, e.g. code snippet how to do something or maybe some not merged experimental branch with some working stuff; so, the next "Implement ..." issue might reuse it  


> **Notify menthor that meeting is passed, provide contacts of 2 decomposition volunteers and github repository URL**.


## Project proceeding

![Meeting daily](./img/meeting-daily.png)

### Everybody must

* have assigned [github issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues) at every moment during the project
* make **daily** status report as a comment to your current issue which answers questions
    * what I **did** yesterday?
    * what I **plan** to do?
    * what **prevents** me from doing what I need?
* commit into branches and **find a reviewer** for your pull requests
* review other's pull requests as a **volunteer** as much as you can

### Everybody should

* do self-review before ask others for review 
* try to [keep PR small](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large): 
    * daily 2-3 files with 20-100 lines is *much* better than 1000+ lines at the end of week (do you think you easely find review volunteer for this?)
* use the basic commit message policy: 
    * 72 chars subject
    * imperative: "Fixed(ing) bug of something" (bad) -> "Fix bug of something" (good)
    * mention issue ID in description
    * use https://gitmoji.dev/ :)
* keep test coverage high (i.e. 60%-80%)
    * if your PR is lowering the test coverage, you have to explain it on code review
* always prefer live communication on code review, do not play ping-pong there


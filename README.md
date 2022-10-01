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


## Start the team & project

* Create your community space (telegram, discord, whatever)
* Organize the first "Team Bootstrap" meeting (<1 hour)

### Team Bootstrap meeting

![Meeting1](./img/meeting1.png)

**Timing**: ~1 hour

**Why**: to meet each other and you need a repository, right?

* give the name to your team; one english word like "assasins", "neptune", "nirvana", etc.
* somebody shares the screen and does the [repository setup](./docs/SETUP_REPO.md)
* everybody do `git clone` of your project and run `./mvnw clean package` to check everything is fine
* vote for two+ colleagues who will do the [initial decomposition](./docs/INITIAL_DECOMPOSITION.md)
* schedule two next meetings:
    * [Bootstrap code session](#bootstrap-code-session): just for that two de-compositors :) 
    * [Grooming and Planning](#grooming-and-planning): for all

> **Notify mentor that meeting is passed, provide contacts of 2 decomposition volunteers and github repository URL**.

### Bootstrap code session

![Meeting 2](./img/meeting2.png)

**Timing**: ~2 hour

**Why**: a way to get a clue about how to make decomposition and try pair programming

* read [Functional requirements](./docs/REQUIREMENTS.md)
* conduct a pair-programming session and create essential services, beans and controllers, see the [initial decomposition](./docs/INITIAL_DECOMPOSITION.md)
* suggestion: try to work on this session in Test Driven way: make some core functional tests at a very high level (only positive scenarios, might be incompleted or having no assertions, etc) and make it working (green)
* make a pull request; the commit subject would be `:rocket: Create essential stubs`
* initiate/schedule [Grooming and Planning](#grooming-and-planning) all-hands meeting

### Grooming and planning

![Meeting3](./img/meeting3.png)

**Timing**: ~2 hour

**Why**: to practice a typical Grooming/Planning scrum meeting at the same time as the first code review

> The hardest meeting. 
> 
> Even do not try to make *ideal* or
> *complete* planning. The goal, for now, is just that everybody 
> must have some clear things to do and assigned Issue.

In fact, this will be the first Code Review session when all the team do code review of `Create essential stubs` pull request.

* two authors share the PR screen and present the code and explain the decomposition
* all the team discussing, if required
* somebody starts to create gitlab issues, use your estimate as suffix of subject, e.g.: `Implement InterviewerTimeSlotService: 8h`
* decide who will be assigned to each task: everybody must be assigned at least to one issue
* merge the reviewed pull request

In the end, you got decomposition by services, so what you need is to create issues in github:

* Implement service FooService
* Implement service BarService

Or maybe starting from scenarios (might be a better way):

* Implement scenario Bla-bla
* Implement scenario Foo-bar

If you are not sure what to do regarding some areas, you might create like:

* Investigate Facebook oauth2 for AuthService
    * better to explain what result would be there, e.g. code snippet how to do something or maybe some not merged experimental branch with some working stuff; so, the next "Implement ..." issue might reuse it  


> **Notify mentor that meeting is passed, provide contacts of 2 decomposition volunteers and github repository URL**.


## Project proceeding

![Meeting daily](./img/meeting-daily.png)

### Everybody must

* have assigned [github issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues) at every moment during the project
* make a **daily** status report as a comment to your current issue which answers questions
    * what I **did** yesterday?
    * what I **plan** to do?
    * what **prevents** me from doing what I need?
* commit into branches and **find a reviewer** for your pull requests
* review other's pull requests as a **volunteer** as much as you can
* be presenter on weekly demos at least once

### Everybody should

* do self-review before asking others for a review 
* try to [keep PR small](https://softwareengineering.stackexchange.com/questions/10793/when-is-a-version-control-commit-too-large): 
    * daily 2-3 files with 20-100 lines is *much* better than 1000+ lines at the end of the week 
    (do you think you easily find a review volunteer for this?)
* use the basic commit message policy: 
    * 72 chars subject
    * imperative: "Fixed(ing) bug of something" (bad) -> "Fix bug of something" (good)
    * mention issue ID in description
    * use https://gitmoji.dev/ :)
* use "Squash and merge" when merging your pull requests (to avoid expose your working commits in `main`)
* keep test coverage high (i.e., 60%-80%)
    * if your PR is lowering the test coverage, you have to explain it at the code review
* always prefer live communication on code review, do not play ping-pong in comments there

### Weekly demos

Every week there are 2 hours meeting with mentor.

One of this hour should be a demo from teams, and second hour is open Q&A session.

Your team have to present short (10 minutes!) demo of team work of previous week.

The demo is sharing the screen and:
* show the tasks were closed this week
* show the tasks which was not closed this week but *expected* to be closed and explain the problems 
* if possible, show some functionality of closed tasks:
   * the best way for backend API functionality demo is to run Postman and demonstrate working requests / scenarios
* show tasks planned to close next week

### Final demo

The final demo is similar to weekly demo but time slot will be 20 minute per team.

For the final demo choose the team speaker and build zthe agenda on:

1. Demonstrating the functionality (Postman)
2. Explain the most successes and failures in this project
3. What you would do better in the next project? 



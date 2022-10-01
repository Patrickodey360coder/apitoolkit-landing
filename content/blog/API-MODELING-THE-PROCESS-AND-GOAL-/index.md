---
title: "API Modeling - The Process And Goals"
date: 2022-05-20T07:34:16+02:00
description: "What does API Modelin entail, what is the process of API Modeling and what is the goal of API modeling?"
---
## API Modeling
API modeling enables us to set up better communication procedures, before going ahead to design our API.  We must be very careful when choosing names for our resources because APIs are permanent. Additionally, it is beneficial to build a shared vocabulary between your product manager and developers, commonly referred to as a ubiquitous language in domain-driven design. Before you choose how to design and construct your API, API modeling provides the answer to the questions of why it exists and what problems it intends to solve.

## The Process Involved in API Modeling
1. Identify the audience for your API
Determining who will use your API is the first step in API modeling. Participants, performers, or users are some other names for them. APIs, in contrast to user interfaces, may be utilized both directly by developers and covertly by end users. Understanding the needs of both sides is crucial. The diverse applications and gadgets that could use our API in various ways are another thing we need to take into account. We might want to be a little bit more explicit when identifying those who are active in accessing the API. Instead of just naming developers as participants, we could want to make a distinction between internal developers, operations engineers, and external developers. The same is true for differentiating end users: regular users, account administrators, and system administrators all utilize different techniques and expect different results when using your API.

2. Determine the desired outcomes
More than your expensive databases or the fine method your code was designed, your end customers are interested in the outcomes that your API makes attainable. For instance, the following may apply to our project management API:
managing a project from start to finish
finding collaborators for a project
Project breakdown into issues
Track the progress of the issue
View unresolved issues
See current projects
Take note that to achieve these results, the activity will probably require one or more steps. Let's break down these outcomes into the steps that turn them into reality as our next step.

3. Diagram the processes necessary to obtain these results.
Steps make up outcomes, and each step is completed by a participant. One or more of your API's endpoints will fuel each stage. Decomposing results into discrete phases necessitates a deeper comprehension of how your API will address practical issues. Make sure to include a subject matter expert ("SME") in your API modeling approach because this knowledge is typically reserved for SMEs.
Using our project management API once more, we can pinpoint some of the actions necessary to achieve our goals:
Project management -> Start a new project
Adding people to a project's team -> A new user account can be created; a user account can be located by email; Add the user in question as a project member.
Break a project down into issues -> Include a project in a problem.
Monitor problem development -> Mark a problem as having begun; Mark a problem as resolved
View outstanding problems -> search for a project; List the project's unresolved problems.
Browse ongoing projects -> List ongoing projects
Even while this isn't an exhaustive list, it does show how our outcomes might be mapped to a single step or require several.

4. Define your API model
You will see the emergence of an API once all the processes have been recognized. Resources and actions will be the components of this API. As we begin the process of designing an API, we may group these to begin identifying the resources and actions that we will require. Just keep in mind that, before going on to the more involved process of API design, the purpose of this phase is to merely capture the APIs and methods to confirm your API needs. Different resources will be discovered in this step like creating a new project, finding a new project etc.

Take note of how our actions and results are assisting us in locating the resources connected to our API. They also assist us in visualizing how the API will be used, giving us a suggestion as to how our ultimate API design might appear. Just keep in mind that these are only potential resources; we might not require all of them or we could require ones that we haven't yet discovered. Making a rough draft of our API before doing the more difficult task of mapping it to HTTP and constructing it is the goal of API modeling.

5. Validate your API model
Validating your API against well-known requirements is the last stage in the modeling process. Your responsibility is to guarantee that your API will satisfy the needs of every user, much like a good quality assurance (QA) team. Verify that your API model can satisfy the indicated demands by using wireframes, user stories, test cases, and other criteria.
Look for missing participants, outcomes, and steps when you validate your APIs. Additionally, you might wish to make a note of APIs that depend on others or that might be heavily used. Even though it's not required, this might help you make certain decisions when you enter the design and development phases.

## The Goal of API Modeling
The goal of API modeling is to transform the specifications for a product into the fundamentals of high-level API architecture. API modeling guarantees that the objectives of developers and end users are met.
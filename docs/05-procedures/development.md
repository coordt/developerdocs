# Development Workflow


## Overall Workflow

Our overall development workflow is pretty simple:

1. Develop
2. Create test instance
3. Quality Assurance
4. Deployment


## Core Project

#. [Create a branch from master](#branching-from-the-core-project).
#. Develop. See the [coding principles](/02-style/principles.md), [coding style](/02-style/coding-style.md), and [model styleguide](/02-style/model-styleguide.md) for guidance.
#. [Periodically commit code](commit-practices)
#. Pull Request for review in github
#. Merge feature back to master


## Branching from the Core Project

Name the branch the Jira issue key without dashes and all lowercase. For example, if the Jira issue key was EDU-6803:

```bash
$ git checkout -b edu6803
```


## Commit Practices

The ideas behind our commiting code practices is based on the assumption:

- Someone may need to find the commit again
- Someone may need to undo this commit
- You will be the "someone" dealing with someone else's code.

Therefore:

- Make the commit easy to find
- Keep the changes as small as possible to make undoing the commit easy

### Guidelines

- Make lots of small commits — they're cheap!
    - *HINT:* Use a GUI like [SourceTree](https://www.atlassian.com/software/sourcetree) to assist in selecting the files and chunks of code.
- Start your commit message with the Jira issue key.
- Make the subject line of what the change is doing/fixing/undoing/etc.
- Use Jira [smart commits](https://confluence.atlassian.com/fisheye/using-smart-commits-298976812.html) to move the ticket to the next phase.
- Write the commit message as if it finishes the sentence: "This commit will ..."

### Examples

```bash
$ git commit -m"EDU-6803 Create new Tron program to fight MCP"
$ git commit -m"EDU-6803 Configure Tron with MCP derezzing instructions"
$ git commit -m"EDU-6803 #qa Derezz the master control program"
```


## New Apps/Projects

1. Create github private/public repo
1. Create the package using the [app skeleton](https://github.com/callowayproject/django-app-skeleton)
1. Develop project
1. Push to github
1. Upload to PyPI
1. Add app/feature to coolorg repository if required
1. Test feature in test instance

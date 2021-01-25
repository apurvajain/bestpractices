# Writing better commit messages

My goal with this blog is to leave you with **NO** excuse for not defining a commit message convention. I’m going to explain the reasons why you should define a git commit message convention, and share detailed instructions to help you move this task from your "to-do" list to "done" in a few simple steps!

## Why use a commit message convention?

- **Better collaboration**
It is important to communicate the nature of changes in your projects in order to foster transparency to a slew of people: existing teammates, future contributors, and sometimes the public and other stakeholders. It’s obvious why a well-formatted Git commit message convention is the best way to communicate context about a change to fellow developers (and their future selves) when requesting a peer code review. A commit message convention also makes it easier to explore a more structured commit history and understand which notable changes have been made between each release (or version) of the project.

- **Squeeze the most out of git utilities**
`$ git log` is a beautiful and useful snippet. A well-organized commit message history leads to more readable messages that are easy to follow when looking through the project history. Suddenly, navigating through the log output become a possible mission! Embracing a commit message convention will also help you properly use other git commands like git blame, git revert, git rebase, git shortlog and other git subcommands.


## Key to writing better commit messages
The most important part of a commit message is that it should be clear and meaningful. In the long run, writing good commit messages shows how much of a collaborator you are. The benefits of writing good commit messages are not only limited to your team, but indeed expand to yourself and future contributors.

Defining a **Commit Message Structure**

```
type(scope): subject 

<body>
<footer>
```

### 1. Type of commit
```
feat:     The new feature being added to a particular application
fix:      A bug fix
style:    Feature and updates related to styling
refactor: Refactoring a specific section of the codebase
test:     Everything related to testing
docs:     Everything related to documentation
chore:    Regular code maintenance
```

### 2. Scope
Provided in parentheses after the type of commit, describing parts of the code affected by the changes or simply the epic name.
```
Example:

feat(claims)
fix(orders)
test(ebill)
```

### 3. Subject
Short description of the applied changes.
- Limit the subject line to 50 characters
- Your commit message should not contain any whitespace errors or punctuation marks
- Do not end the subject line with a period
- Use the present tense or imperative mood in the subject line

```
feat(claims): add claims detail page
fix(orders): validation of custom specification
```

### 4. Body (Optional)
Use the body to explain what changes you have made and why you made them.
- Separate the subject from the body with a blank line
- Limit each line to 72 characters
- Do not assume the reviewer understands what the original problem was, ensure you add it
- Do not think your code is self-explanatory

```
fix(orders): missing validation of custom product

### Summary
No validation error thrown on entering string for length field
while customsing product

### Steps to reproduce
1. Login and go to Order Placement page
2. Select any product
3. Try customsing it by entering any string in "length" field

### Fixes
Add validation to length text input to only accept float values
```


### 5. Footer (Optional)
Use to refrence issues affected by the code changes.

```
fix(orders): validation of custom specification

### Summary
No validation error thrown on entering string for length field
while customsing product

### Steps to reproduce
1. Login and go to Order Placement page
2. Select any product
3. Try customsing it by entering any string in "length" field

### Fixes
Add validation to length text input to only accept float values

Resolves: #123
See also: #456, #789
```

## General Tips
- **Make Small, Single-Purpose Commits**
A commit should be a wrapper for related changes. For example, fixing two different bugs should produce two separate commits. Small commits make it easier for other developers to understand the changes and roll them back if something went wrong. Also, if separate policies govern your projects or codes, it is an excellent idea to keep them in distinct code lines for proper source code management and better housekeeping.

- **Commit Early & Often**
Committing early and often keeps your commits small and helps you commit only related changes. Moreover, it allows you to share your code more frequently with others and avoid having merge conflicts. So, each time you change something, commit – whether it is renaming a file or something even more mundane, don’t think but commit until it becomes second nature.

- **Test Your Code Before You Commit**
Resist the temptation to commit something that you «think» is completed. Test it thoroughly to make sure it really is completed and has no side effects (as far as one can tell). While committing half-baked things in your local repository only requires you to forgive yourself, having your code tested is even more important when it comes to pushing/sharing your code with others.

- **Agree on A Workflow**
Git lets you pick from a lot of different workflows: long-running branches, feature branches, merge or rebase, git-flow, etc. Which one you choose depends on a couple of factors: your project, your overall development and deployment workflows and (maybe most importantly) on your preferences. However you choose to work, just make sure to **agree on a common workflow** that everyone follows.
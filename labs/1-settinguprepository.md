# 1 Migrating your repository from Azure DevOps
This hands-on lab has the goal to show you how you can migrate an existing Azure DevOps git repository to GitHub. At the end of this hands on lab, your GitHub repository (`https://github.com/Microsoft-Bootcamp/attendee-<yourGitHubhandle>`) will contain a migrated copy of an Azure DevOps repository. This is the first step that needs to be finished for all the other labs to succeed, as you will use this repository to perform the other hands on labs. Good luck! 👍

This hands on lab consists of the following steps:
- [Migrating your repository from Azure DevOps](#11-migrating-your-repository-from-azure-devops)
  - [Acquire the Azure DevOps Git repository URI](#acquire-the-azure-devops-git-repository-uri)
  - [Import the repository](#import-the-repository)
  - [Import the repository - Using GUI](#import-the-repository---using-gui)
- [Create a Branch Rule](#create-a-branch-rule)
- [If time permits: enforce CODEOWNERS review](#if-time-permits-enforce-codeowners-review)

## Acquire the Azure DevOps Git repository URI
For this bootcamp, we have prepared a public Azure DevOps repository for you and created a private GitHub repository for you. Before you can clone the existing repository, you'll need an URI that points to the existing repository. This URI represents the source of the repo you're going to copy. 

**For this bootcamp, please use the following URL: [https://dev.azure.com/microsoft-bootcamp/BootcampExercises](https://dev.azure.com/microsoft-bootcamp/BootcampExercises)**

Start with retrieving the Clone URL of the Azure DevOps repository you want to migrate.
1. From your web browser, open the team project for your Azure DevOps organization and choose `Repos`, then `Files`.

![get clone uri](https://docs.microsoft.com/en-us/azure/devops/repos/git/media/clone-repo/repos-files.png?view=azure-devops)

2. Select `Clone` in the upper right. Copy this URL into the clipboard or store it in a place where you can find it easily. You can't clone a repo without a clone URL.

![copy clone uri](https://docs.microsoft.com/en-us/azure/devops/repos/git/media/get_clone_url.gif?view=azure-devops) 

## Import the repository
Next, we'll import the Azure DevOps repository in your GitHub repository. There are two ways to achieve this, you'll use one of both: 
2. [Import the repository - Using GUI](#import-the-repository---using-gui)

Navigate to your repository on GitHub, it should still be completely empty and offer the ability to "Import Code" at the bottom of the page:

![Import Code](../images/import-code.png) 

The next screen will prompt you for the source to import. Use the clone URL for the Azure DevOps Repository you copied in the previous step:

![Import Code - Set Source](../images/import-code-provide-source.png)

Verify that you've selected the correct target repository and click "begin import".

GitHub will now automatically import and optimize the repository and should finish in a moment:

![Import Code - Finished](../images/import-code-finished.png)

Now clone your repository to your local machine with the CLI or with your favorite git client:

```
git clone https://github.com/Microsoft-Bootcamp/attendee-<your-github-handle>.git
```

You can now skip directly to [Create a Branch Rule](#create-a-branch-rule).

# Create a Branch Rule
By now your repository at GitHub has content and we can now protect our branches against unwanted direct updates. This is a very common setup in the enterprise.
In this excursive we will create a branch rule that prevents you to commit to the main branch direct and require you to create a pull request.

In your GitHub repo go to the settings tab and click the branches option as shown here:

![branch protection rules](../images/branch-protection-rules.png)

Now add a new rule and define which branch you want to protect. e.g. provide the pattern name `main`
Next you select the following options:
1. `Require a pull request before merging`
2. `Do not allow bypassing the above settings`

Now save this Branch Rule and see if it works.

To check if it works, create a change on one of the files that is in the main branch. Just use the portal to make the change and verify that the moment you want to commit the change you see the below indicator that you need to create a new branch before you can commit:

![create branch before commit](../images/branch-before-commit.png)

Now, create a new branch and create a pull request that enables the code review from someone else in your organization/repository.

# If time permits: enforce CODEOWNERS review

If you want to enforce certain teams can only approve parts of the codebase, like a web development team for all the web application code and a docs team for the documentation, you can use the code owners file. We can enforce the code owners need to be part of the review process by adding this to the Branch Protection Rule.
There is already a CODEOWNERS file in the repo that you migrated. It has the following contents:

```
# Example, any change in this repo 
# will require approval from @vriesmarcel
# * @vriesmarcel

# Any change inside the `/Documents` directory
# will require approval from anyone in the organization fluentbytes and the team docsteam
# /docs @dkalmin
# Create your own rules below this line without the # sign
```
Now change the file so it defines the folder `/code/src/AttendeeSite/` has a team as owner. All attendees of todays technical workshop are part of a team that we set up. Assign to one of your trainers, or to your attendee instructor (eg @dkalmin).

After setting up the file, commit it to the main branch.
Next go to the branch protection rules and for the main branch select the option `Require review from Code Owners`

Save the branch protection rule and make a change to e.g. the startup.cs file and see if you can commit the changes to the main branch. Ask one of the other attendees to review the pull request you created and see if it then is allowed to approve and merge the pull request. Please use the teams chat to find a colleague who can approve your pull request. If you can't find anyone, ask the instructors. 

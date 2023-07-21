# 6 Migrating pipelines from Azure DevOps to GitHub Actions using GitHub Actions Importer 
In this hands-on lab you will get a glance at the tooling that is built to forecast, plan, and facilitate migrations from your current CI/CD tool to GitHub Actions. This tool is called **GitHub Actions Importer (GAI)**.

This hands on lab consists of the following steps:
- [Add Personal Access Tokens To Codespaces](#add-personal-access-tokens-to-codespaces)
- [Bootstrap GitHub Actions Importer into your Codespace](#bootstrap-github-actions-importer-into-your-codespace)
- [Generate GitHub a Personal Access Token](#generate-a-github-personal-access-token)
- [Configure Actions Importer](#configure-actions-importer)
- [Verify your environment](#verify-your-environment)
- [Run an audit on the existing Azure DevOps project](#run-an-audit-on-the-existing-azure-devops-project)
- [Execute the migration](#execute-the-migration)
- [If time permits create a custom mapping](#if-time-permits-create-a-custom-mapping)
- [Continue learning GitHub Actions Importer](#continue-learning-github-actions-importer)


## Add Personal Access Tokens To Codespaces
1. Work inside your current repository `Canarys-Bootcamp/attendee-<your-github-handle>`
2. Click the `Settings` tab
3. Click `Secrets` then `Codespaces`
4. Click `New Repository Secret`
5. In the Name field type `AZURE_DEVOPS_ACCESS_TOKEN`
6. Paste in **ASK INSTRUCTOR**
7. Click `Add Secret`


## Bootstrap GitHub Actions Importer into your Codespace
1. Work inside your current repository `Canarys-Bootcamp/attendee-<your-github-handle>`
2. Open the `.devcontainer` folder
3. Open the `devcontainer.json` file
4. Click `Edit`
5. Replace the complete file contents with the below:
```
{
  "name": "Codespace to perform GitHub Actions Importer Labs",
  "remoteEnv": {
    "INSTALLATION_TYPE": "labs"
  },
  "hostRequirements": {
    "cpus": 4,
    "memory": "8gb",
    "storage": "32gb"
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-azuretools.vscode-docker"
      ]
    }
  },
  "postCreateCommand": "gh extension install github/gh-actions-importer || echo 'Could not auto-build. Skipping.' "
}
```

## Generate a GitHub Personal Access Token
1. To do so, navigate to your GitHub Settings - click your profile photo and go to Settings or follow [this link](https://github.com/settings/profile).
2. Go to `Developer Settings`
3. Go to `Personal Access Tokens`
4. Click `Generate new token`. If required, provide your password.
5. Select at least the scopes `read packages` and `workflow`. Optionally, provide a text in the field `Note` and change the `Expiration`. 
6. Click `Generate token`
7. Your token is now generated. Make sure to copy your token, as you will not be able to retrieve it later on.

## Configure Actions Importer
3. Run the `configure` CLI command:
      - Select the `TERMINAL` tab from within the codespace terminal.
      - Run the following command: `gh actions-importer configure`.
      - Use the down arrow key to highlight `Azure DevOps`, press the spacebar to select, and then press enter to continue.
      - At the GitHub Personal Access Token prompt, enter the GitHub PAT generated in step 3 and press enter.
      - At the GitHub URL prompt, enter the GitHub instance URL or press enter to accept the default value (`https://github.com`).
      - At the Azure DevOps token prompt, ASK INSTRUCTOR FOR Azure DevOps Access token, paste it in and press enter.
   - At the Azure DevOps URL prompt, enter your Azure DevOps URL or press enter to accept the default value (`https://dev.azure.com`).
      - At the prompt, enter your Azure DevOps organization name of `microsoft-bootcamp` and press enter.
      - At the prompt, enter your Azure DevOps project name of `BootcampExercises`. and press enter.

         ```console
         $ gh actions-importer configure
         ✔ Which CI providers are you configuring?: Azure DevOps
         Enter the following values (leave empty to omit):
         ✔ GitHub handle used to authenticate with the GitHub Container Registry: mona
         ✔ Personal access token for GitHub: ***************
         ✔ Base url of the GitHub instance: https://github.com
         ✔ Personal access token for Azure DevOps: ***************
         ✔ Base url of the Azure DevOps instance: https://dev.azure.com
         ✔ Azure DevOps organization name: :organization
         ✔ Azure DevOps project name: :project
         Environment variables successfully updated.
         ```

## Verify your environment

To verify your environment is configured correctly, run the `update` CLI command. The `update` CLI command will download the latest version of GitHub Actions Importer to your codespace.

1. In the codespace terminal, run the following command:

   ```bash
   gh actions-importer update
   ```

2. You should see a confirmation that you were logged into the GitHub Container Registry and the image was updated to the latest version.

   ```console
   $ gh actions-importer update
    Updating ghcr.io/actions-importer/cli:latest...
    ghcr.io/actions-importer/cli:latest up-to-date
   ```

## Run an audit on the existing Azure DevOps project

1. Navigate to the codespace terminal.
2. Run the following command from the root directory:

    ```bash
    gh actions-importer audit azure-devops --output-dir tmp/audit
    ```

    __Note__: The Azure DevOps organization and project name can be omitted from the `audit` command because they were persisted in the `.env.local` file in the [configure lab](./1-configure.md). You can optionally provide these arguments on the command line with the `--azure-devops-organization` and `--azure-devops-project` CLI options.

3. The command will list all the files written to disk in green when the command succeeds.

## Inspect the output files

1. Find the `audit_summary.md` file in the file explorer.
2. Right-click the `audit_summary.md` file and select `Open Preview`.
3. This file contains details that summarize what percentage of your pipelines were converted automatically.

# Execute the migration
  
Now we are going to migrate one of the successful pipelines. 

Now, we can run the command line and need to pass it the pipeline command. This command also requires to pass in a --target-url, which is the GitHub repo you are targeting. This is the location `https://github.com/Canarys-Bootcamp/<your-repo-name>`.  

The Bootcamps Azure DevOps pipeline's definition-id is 54.

1. Navigate to the codespace terminal.
2. Run the following command from the root directory:
```
gh actions-importer migrate azure-devops pipeline --target-url https://github.com/Canarys-Bootcamp/<your-repo-name> --pipeline-id 54 --output-dir tmp/migrate
```

You will find a similiar result to:
  
```
@dkalmin ➜ /workspaces/attendee-dkalmin-1-17-18 (main ✗) $ gh actions-importer migrate azure-devops pipeline --target-url https://github.com/Canarys-Bootcamp/attendee-dkalmin-1-17-18 --pipeline-id 54 --output-dir tmp/migrate
[2023-01-18 16:48:11] Logs: 'tmp/migrate/log/valet-20230118-164811.log'         
[2023-01-18 16:48:17] Pull request: 'https://github.com/Canarys-Bootcamp/attendee-dkalmin-1-17-18/pull/6'
@dkalmin ➜ /workspaces/attendee-dkalmin-1-17-18 (main ✗) $ 
 ```

The last line shows you a pull request that got created on the target repository that contains the migrated workflow.
This will look as follows:

![pull request](../images/workflow-pullrequest.png)

Now, inspect the pull request and the action workflow that is part of the pull request.
Merge the pull request and run the workflow manually.

Success! The Action should run and run successfully

### If time permits create a custom mapping

#### Lets fail a PR
Run the following command to execute the migration on a pipeline that will fail on the PR checks build run:
```
gh actions-importer migrate azure-devops pipeline --target-url https://github.com/Microsoft-Bootcamp/<your-repo-name> --pipeline-id 53 --output-dir ./migrate
```

You will find the following results with a NEW pull request:
  
```
[2022-04-19 16:49:46] Pull request: 'https://github.com/Microsoft-Bootcamp/dkalmin-4-18-test2/pull/1' 
```
The result is a new PR in GitHub. The build that runs will fail. You can manually fix the workflow in the next activity.

In this activity, you will learn to create a custom plugin that transforms some of the existing migration mapping and replace it by your own mapping. 
For this you need to start coding in Ruby. 
> Useful links on Ruby: [https://www.ruby-lang.org/en/](https://www.ruby-lang.org/en/) and [https://ruby-doc.org/](https://ruby-doc.org/).

#### Creating a custom mapping
To create a mapping, you need to create a Ruby file that looks as follows:
``` ruby
transform "azuredevopstaskname" do |item|
   # your ruby code here that produces output
  end
```  
Create a new folder called `plugin` under the root folder in your repository. In there create a file called `DotNetCoreCLI.rb`.

If we look at the transformation that was generated for our CI pipeline, we have seen the transformation does not take into account the fact we specified a wildcard pattern to match any of the csproj files we might have in our repo. This now results in a workflow that fails the build if we accept the default mapping.

Let's see if we can fix this by providing an alternative.

We start by overriding the `DotNetCoreCLI@2` task from the Azure DevOps pipeline

This results in the following code:
``` ruby
transform "DotNetCoreCLI@2" do |item|
   # your ruby code here that produces output
  end
```  
The parameter item is a collection of items than contain the properties of the original task that was retrieved from Azure DevOps.
In our case we can see in the yaml the properties that are set are e.g. `command` and `projects`.

Lets make a code change to the ruby file and change it to this:
``` Ruby
transform "DotNetCoreCLI@2" do |item|
  projects = item["projects"]
  command = item['command']
  run_command = []
  
        if(projects.include?("$"))
          if(command.nil?)
            command = "build"
          end
           run_command << "shopt -s globstar; for f in ./**/*.csproj; do dotnet #{command} $f #{item['arguments'] } ; done"
        else
            run_command << "dotnet #{command} #{item['projects']} #{item['arguments'] }"
       end 
    {
      shell: "bash",
      run: run_command.join("\n")
    }
end
```
And then run the actions-importer command line where we pass in the custom mapping like this, be sure to replace the path to the repo:
```
gh actions-importer migrate azure-devops pipeline --target-url https://github.com/Microsoft-Bootcamp/<your-repo-name> --pipeline-id 53 --custom-transformers plugin/DotNetCoreCLI.rb --output-dir ./migrate
```
Navigate to the new pull reguest generated in your GitHub repository. The build check will now run successfully and you can merge the PR!

### Continue learning GitHub Actions Importer
Attending this GitHub Technical Bootcamp means that you have been granted access to the GitHub Access Importer cli for use going forward. To learn more about the additional features and CI/CD providers supported from the GAI please visit the [GitHub Actions Importer labs](https://github.com/actions/importer-labs). 

The GitHub Actions Importer labs is an open source repository that will guide you through the following migration scenarios:
- [Migrating from Azure DevOps to GitHub Actions](https://github.com/actions/importer-labs/blob/main/azure_devops/readme.md)
- [Migrating from CircleCI to GitHub Actions](https://github.com/actions/importer-labs/blob/main/circle_ci/readme.md)
- [Migrating from GitLab to GitHub Actions](https://github.com/actions/importer-labs/blob/main/gitlab/readme.md)
- [Migrating from Jenkins to GitHub Actions](https://github.com/actions/importer-labs/blob/main/jenkins/readme.md)
- [Migrating from Travis CI to GitHub Actions](https://github.com/actions/importer-labs/blob/main/travis/readme.md)

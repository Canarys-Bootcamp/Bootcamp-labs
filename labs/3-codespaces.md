# 3 Creating your first Codespace

In this hands-on lab your will create your first GitHub Codespace and learn how you can use Codespaces to work from anywhere from a browswer as well as . If you like more background information, please refer to the [GitHub Codespaces](https://docs.github.com/en/codespaces) pages on GitHub Docs. Good luck! 👍

This hands on lab consists of the following steps:
- [Creating first codespace](#creating-first-codespace)
- [Develop and push in a codespace](#develop-and-push-in-a-codespace)
- [Add devcontainer and devcontainer.json file](#add-devcontainer-and-devcontainer-json-file)

## Creating first codespace
1. Work inside your current repository `Microsoft-Bootcamp/attendee-<your-github-handle>`
2. Under the repository name, use the  Code drop-down menu, and in the Codespaces tab, click Create codespace on main.

![Screen Shot 2022-04-18 at 7 40 38 AM](https://user-images.githubusercontent.com/26442605/163824575-5516195b-21bc-4e5d-a7ad-6699bc34b88f.png)

3. WAIT for postCreatecommand to finish.

![Screen Shot 2022-04-18 at 8 40 38 AM](https://user-images.githubusercontent.com/26442605/163833596-499f4103-f034-4d9e-a932-b7b65ef271f8.png)


4. Once your codespace is created, your repository will be automatically cloned into it. Now build your application in your codespace.
5. In the Terminal of the Visual Studio code run the following
```
cd code
dotnet build
```
6. Verify your dotnet project build correctly. It should look like the following.
![Screen Shot 2022-04-18 at 8 45 31 AM](https://user-images.githubusercontent.com/26442605/163834229-88b7780b-08da-4064-8e16-56db770da744.png)

## Develop and push in a codespace
1. In the code folder open the readme.md file
2. Add the following in the readme.md file
```
Hello! From the codespace.
```
3. Save the file!
4. From the terminal run the following commands.
```
git add *
git commit -m  "adding from codespace"
git push
```
5. You have now pushed code to your main repo! Codespace should look like this.

![Screen Shot 2022-04-18 at 8 53 39 AM](https://user-images.githubusercontent.com/26442605/163835432-dff0f473-e4b9-43a0-b1fa-98a83ce9cd69.png)

6. Your GitHub Repo should look like this.

![Screen Shot 2022-04-18 at 8 55 02 AM](https://user-images.githubusercontent.com/26442605/163835565-bc3176f7-8517-402d-9a64-7c6ae3c20f11.png)


## Add devcontainer and devcontainer json file
1. Work inside your current repository `Microsoft-Bootcamp/attendee-<your-github-handle>`
2. Add a .devcontainer folder and .devcontainer.json in the root of your Repo
3. Click "Add File"
4. Click "Create New File"
5. Type or copy paste.
```
.devcontainer/devcontainer.json
```
6. In the body of the file paste the following.
```
{
  "name": "Codespace to bootstrap valet in a Codespace",
  //Use base codespace image then pull Valet on postCreateCommand,  
  "image": "mcr.microsoft.com/vscode/devcontainers/universal:linux",
  "remoteUser": "codespace",
	"overrideCommand": false,
	"mounts": ["source=codespaces-linux-var-lib-docker,target=/var/lib/docker,type=volume"],
	"runArgs": [
		"--cap-add=SYS_PTRACE",
		"--security-opt",
		"seccomp=unconfined",
		"--privileged",
		"--init"
	],
	
	// Add the IDs of extensions you want installed when the container is created.
	"extensions": [
		"GitHub.vscode-pull-request-github",
		"ms-vscode.azure-account",
		"ms-vscode.azurecli",
		"ms-azure-devops.azure-pipelines",
		"ms-azuretools.vscode-docker",
		"ms-vscode.powershell"
	]
}
```
7. Click "Commit New FIle" directly on the main branch
8. Create a new codespace and view the added extensions
## If time permits: Customize your worksapce

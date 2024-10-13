To set up Visual Studio Code (VS Code) with GitHub, you need to configure Git in VS Code, connect it to your GitHub account, and clone or create repositories. Here’s a step-by-step guide:

### 1. **Install Git**
Ensure that Git is installed on your system. If it's not installed:
- Download Git from [https://git-scm.com](https://git-scm.com) and install it.

### 2. **Install Visual Studio Code**
- Download and install Visual Studio Code from [https://code.visualstudio.com](https://code.visualstudio.com).

### 3. **Configure Git in VS Code**
Once both Git and VS Code are installed:
1. Open **VS Code**.
2. Go to the **Terminal** and check if Git is installed by typing:
   ```bash
   git --version
   ```
   If Git is installed correctly, this will return the installed version number.

### 4. **Configure Git User Info**
You need to set your Git username and email that will be associated with your commits:
```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### 5. **Install GitHub Extension for VS Code**
1. Open VS Code.
2. Go to the **Extensions** panel on the left side (or press `Ctrl+Shift+X`).
3. Search for **GitHub Pull Requests and Issues** extension by Microsoft.
4. Install the extension.

### 6. **Sign in to GitHub**
1. After installing the extension, click on the **Source Control** tab (or press `Ctrl+Shift+G`).
2. Click on **Sign in to GitHub**.
3. VS Code will prompt you to log in. Follow the prompts to sign in through your GitHub account.
4. Alternatively, you can generate a Personal Access Token from GitHub and use it to authenticate:
   - Go to GitHub, click on your profile, and navigate to **Settings** > **Developer settings** > **Personal access tokens**.
   - Generate a token and use it when prompted by VS Code.

### 7. **Cloning a GitHub Repository**
1. Click on the **Source Control** tab in VS Code.
2. Click on the **Clone Repository** button.
3. Paste the URL of your GitHub repository and choose the folder where you want to save it.
4. Once cloned, VS Code will ask if you want to open the repository; click **Open**.

### 8. **Create a New Repository**
1. Go to **View > Command Palette** (`Ctrl+Shift+P`).
2. Type **Git: Initialize Repository** and select the folder you want to initialize as a Git repository.
3. After the repository is initialized, you can push it to GitHub by running:
   ```bash
   git remote add origin https://github.com/your-username/your-repository.git
   git push -u origin master
   ```

### 9. **Commit and Push Changes**
1. After making changes, go to the **Source Control** tab.
2. Enter a commit message in the text box at the top.
3. Click the checkmark icon to commit.
4. To push the commit to GitHub, click on the three dots at the top of the Source Control panel and select **Push**.

That’s it! You've successfully set up GitHub with VS Code.
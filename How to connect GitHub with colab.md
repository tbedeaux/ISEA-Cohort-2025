# Connect GitHub with Google Colab

---

## Setup GitHub

1. Create a GitHub account:
    * Go to [GitHub](https://github.com/login) and sign up for a new account.
    * Verify the email address to activate the account.

2. Join the project organization:
    * Accept the organization invitation

3. Generate a Personal Access Token (PAT):
    * Go to *Settings* -> *Developer settings* -> *Personal access tokens* -> *Tokens (Classics)* or *Fine-grained personal access tokens* -> *Generate new token*.
    * Generate a new token by following the instructions with the appropriate scopes (e.g., repo, workflow).
        - Provide a descriptive name for the token that helps us remember what the token is used for, such as "Google Colab Integration".
        - Choose an expiration date for the token. Options range from 30 days to no expiration. Consider the security needs when choosing.
        - Check the boxes for the scopes or permissions we need the token to have. For most integrations that involve reading and writing to repositories, select: *repo* (covers private repositories). *workflow* (allows you to manage GitHub Actions workflows).
        - Once we have configured all options, scroll down and click the *Generate token* button at the bottom of the page.
        - GitHub will display the new personal access token.
    * Copy and save the token securely in a *.txt* file as it is shown only once.
    * We can now use this token in place of a password for command line Git operations, API calls, or in applications like Google Colab that require GitHub access.

## Google Colab

1. Ensure that we have a Google account to use [Colab](https://colab.research.google.com/notebook#).

---

## Reading data from private  GitHub repos into Colab notebook

1. Add GitHub PAT in Google Colab Secrets tab:
    * Click -> Add new secret -> add Name (e.g., "github") - > Value (add your PAT) -> Action (click show)
2. Trun the key on to give the notebook access permission
3. Run the following on a cell to clone the repo on the currect colab session:
    ```
    from google.colab import userdata
    token = userdata.get('github')
    %cd /content/
   !git clone  https://your github username:{token}@github.com/organization name/repo name.git
    ```
---

## Push a file from Google Colab to a GitHub repo

1. Configure Git in Google Colab:
    ```
    !git config --global user.email "you@example.com"
    !git config --global user.name "Your Username"
    ```

2. Clone the repository in the currect Colab session following previous steps 2. and 3.
    ```
    from google.colab import userdata
    token = userdata.get('github')
    %cd /content/
   !git clone  https://your-github-username:{token}@github.com/organization-name/repo-name.git
    ```

3. Change directory path to the repo
    ```
    import os
    os.chdir('/content/repo-name')
    ```

4. Add/edit anything in your folder in the repo
    ```
    file_name = 'contributors/member_1/example.txt'
    !git add {file_name}
    ```

5. Commit and push changes to the main/master branch
    ```
    repo_url = 'https://github.com//organization-name/repo-name'
    !git commit -m "Add example file"
    !git push https://{token}@{repo_url.split('//')[1]}
    ```
OR

6. Commit and push changes to another branch (since it is a collaborative repo)
    ```
    #create a new brunch
    !git checkout -b new-branch-name 

    # OR switch to an existing branch
    !git checkout branch-name

    file_name = 'contributors/member_1/example.txt'
    !git add {file_name}

    #commit the changes
    !git commit -m "Add example file"

    #push the changes to the current branch
    !git push origin HEAD
    ```

---

### Resources

- [Using Google Colab with GitHub](https://colab.research.google.com/github/googlecolab/colabtools/blob/master/notebooks/colab-github-demo.ipynb)

- Colab can load public github notebooks directly, with no required authorization step.

- http://colab.research.google.com/github will give us a general github browser, where we can search for any github organization or username. OR, http://colab.research.google.com/github/googlecolab/ will open the repository browser for the googlecolab organization. Replace googlecolab with any other github org or user to see their repositories.

- Anybody can open a copy of any github-hosted notebook within Colab through *Open in Colab* badge. The markdown for the badge is the following which we can put it in the Text cell of the notebook:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/googlecolab/colabtools/blob/master/notebooks/colab-github-demo.ipynb)

- If we are getting error after clicking on the badge, check the notebook address and fix it. For example, if the colab notebook is here: https://github.com/your-org-name/project-repo/blob/main/contributors/member1/member1_colab_template.ipynb, make sure that the address in the badge markdown points to the same address. To check that, click on *Raw* on the right top corner of the notebook and check the address. If the address is wrong, correct it and then click commit (preferably in your branch).

---

# Managing Collaborative Workflow on GitHub

There are many ways to contribute to a project and two of those are higlighted below.
We encourage participants to practice either one during an event and many open source projects have similar guides to contribute.

The two flows are:
* Contributing directly with a branch
* Contributing through a fork

### Terminology
Before we dive into the workflows, here some essential terms and links to definitions:
* [Branch](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#branch)
* [Clone](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#clone)
* [Fork](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#fork)
* [Pull Request or short PR](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#pull-request)
* [Push](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#push)

## Contributing directly with a branch

The following explains the steps to [contribute directly to a repository using
a branch](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-flow).

In a nutshell, here the flow with the Google Colab where you clone the repo:

1. Clone the repository on the currect Colab session

   :bangbang: **NOTE** you are cloning the original repository and not your fork.

   - Configure Git in Google Colab:
        ```
        !git config --global user.email "you@example.com"
        !git config --global user.name "Your Username"
        ```
    - Run the following in a code cell (assuming you have added PAT into the Secrets as mentioned above):
        ```
        from google.colab import userdata
        token = userdata.get('github')
        %cd /content/
        !git clone  https://your-github-username:{token}@github.com/organization-name/repo-name.git
        ```
2. Change directory path to the repo
    ```
    import os
    os.chdir('/content/repo-name')
    ```

4. Add/edit anything in your folder in the repo
    ```
    file_name = 'contributors/member_1/example.txt'
    !git add {file_name}
    ```
5. Create a branch

   - Choose a unique name that is not already taken in the repository and helps other collaborators to identify your work.

        ```
        #create a new brunch
        !git checkout -b new-branch-name 

        # OR switch to an existing branch
        !git checkout branch-name
        ```

6. Make changes within the new branch

   - With this sample repository, you have to create a folder within **`contributions`** folder with your GitHub username as the folder name. Use a new markdwon file (`.md`) or text file (`.txt`) file to practice git with.
    ```
    file_name = 'contributors/member_1/example.txt'
    !git add {file_name}
    ```

7. Commit the changes
    ```
    #commit the changes with a descriptive message explaining the changes
    !git commit -m "Add example file"
    ```

8. Push the changes to the your branch
    ```
    !git push origin HEAD
    ```

9. Create a pull request from your new branch

   - Go to the repository page on github.com. There, you will see a banner to create the pull request.

10. Go to the pull request tab and create a new pull request and follow the instructions to pull and merge with the main branch.
    - Once a branch is ready, the changes should be merged into the main branch via a pull request. IT allow team members to review code, discuss it, and make additional commits if necessary before the changes are merged.

        - Create a pull request from the GitHub website.
        - Assign team members to review the pull request.
        - After approval, merge the pull request into the main branch.

11. (Optional) Clean up and delete the branch on the Colab.

   - Once your pull request is merged to the `main` branch, it is a good practice to delete the created branch. This has to be done in two places, once on the pull request page on github.com and the other on your local repository or google colab.

   - For github.com, see the official documentation on
   [how to delete a branch](https://docs.github.com/en/github/administering-a-repository/managing-branches-in-your-repository/deleting-and-restoring-branches-in-a-pull-request#deleting-a-branch-used-for-a-pull-request) after it has been merged.

12. Go back to the `main` branch
    ```
    !git checkout main
    ```

### Note:

* To minimize conflicts and keep up-to-date with the main branch (often main or master), each member should frequently pull the latest changes before they start working and also before they push new changes. This is crucial when multiple people are editing similar parts of the project.

    ```
    #Before starting work, pull the latest changes from the main branch
    !git checkout main
    !git pull origin main

    #Switch to your feature branch (create one if it does not exist)
    !git checkout -b branch-name  # Create and switch
    # or
    !git checkout branch-name  # Switch to an existing branch

    #after making changes, pull again before pushing to resolve any potential conflicts
    !git pull origin main
    #Resolve any conflicts that arise
    ```

* If conflicts occur during the pull (which is likely if others have modified the same files), team members will need to resolve these conflicts locally before committing their changes. Git will mark the files that have conflicts, and developers need to manually edit the files to resolve them.

* Commit and Push Changes Regularly. Regular commits with clear, descriptive messages help keep track of changes and make it easier to understand the history of the project.
    ```
    !git add .
    !git commit -m
    #Pull again to ensure there are no new changes before you push
    !git pull origin main
    #Push your changes to your branch
    !git push origin branch-name
    ```

* **Best Practices:**
    * Keep everyone in the loop about what you are working on and any issues you encounter.
    * Document both your code and your Git processes.
    * Commit Often, Push Frequently.
---

# (Optional) Learning Git Repo Version Control System

---

There are many ways to contribute to a project and two of those are higlighted below.
We encourage participants to practice either one during an event and many open source projects have similar guides to contribute.

The two flows are:
* Contributing directly with a branch
* Contributing through a fork

## Git workflows

### Which one to choose?
The way you can contribute and choose one of the above flows depends on your access
privileges to a repository. You can use both flows, if you are part of the organization
that owns the repository. This is the case when you participate in the hackweek and
we added you to the GitHub organizaion. Once you are not part of the organization or
if you want to contribute to a repository that is on someone else's personal account,
then you need to use a fork.

### Terminology
Before we dive into the workflows, here some essential terms and links to definitions:
* [Branch](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#branch)
* [Clone](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#clone)
* [Fork](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#fork)
* [Pull Request or short PR](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#pull-request)
* [Push](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-glossary#push)

## Contributing through a branch

The following explains the steps to [contribute directly to a repository using
a branch](https://docs.github.com/en/github/getting-started-with-github/quickstart/github-flow).

In a nutshell, here the flow:

1. Clone the repository on your local computer

   :bangbang: **NOTE** This is different from the above as you are cloning the original
   repository and not your fork.

   Command:
   ```shell
   git clone https://github.com/uwhackweek/learning-git
   ```

1. Create a branch

   Choose a unique name that is not already taken in the repository and helps
   other collaborators to identify your work. In this case we are adding a branch
   with name 'feature_xyz'

   ```shell
   # The '-b' is indicating a new branch with the name `feature_xyz`
   # It will also change to that.
   git co -b feature_xyz
   ```

   Sample output:
   ```shell
   Switched to a new branch 'feature_xyz'
   ```

1. Verify you are on the new branch

    ```shell
    # Show all branches of the repository, the current active has the `*`
    git branch
    ```

   Output:
    ```shell
    main
    * feature_xyz
    ```

1. Make changes within the new branch

   With this practice repository, you have to create a folder within
   **`contributions`** folder with your GitHub username as the folder name. Use
   a new markdwon file (`.md`) or text file (`.txt`) file to practice git with.
   No other file types or folders are allowed outside your designated folder
   with this practice run.

1. Commit the changes

   ```shell
   # Add the new files
   git add .
   # Commit the changes
   git commit -m "My new files"
   ```

1. Push the changes to your forked repository

   ```shell
   git push origin feature_xyz
   ```

1. Create a pull request from your new branch

   Go to the repository page on github.com. There, you will see a banner to create
   the pull request.

1. Go to the pull request tab and celebrate!

   ```shell
    https://github.com/uwhackweek/learning-git/pulls
    ```

1. Clean up and delete the branch locally and on the website.

   Once your pull request is merged to the `main` branch, it is a good practice
   to delete the created branch. This has to be done in two places, once on
   the pull request page on github.com and the other on your local repository.

   For github.com, see the official documentation on
   [how to delete a branch](https://docs.github.com/en/github/administering-a-repository/managing-branches-in-your-repository/deleting-and-restoring-branches-in-a-pull-request#deleting-a-branch-used-for-a-pull-request)
   after it has been merged. In this practice repository, we have setup a rule
   that will take care of this step for you.

   Locally, however, you must still do this yourself and here the steps you
   need to execute within your local repository:

   ```shell
   # Go back to the `main` branch
   git checkout main
   # Get the latest changes that include your pull request
   git pull
   # Delete the branch. Here, our practice run branch `feature_xyz`
   # The -d flag is a safety option that only allows fully merged branches
   # to be deleted.
   git branch -d feature_xyz
   ```

## Contributing through a fork

An in-depth description for the forking workflow can
[be found here](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

---

### Resources

- [uwhackweek tutorial on Git Version Control](https://github.com/uwhackweek/learning-git/blob/main/README.md)

- [Learn Git Fork & Pull Request Workflow](https://jarednielsen.com/learn-git-fork-pull-request/)

- [Git cheat sheet](https://training.github.com/downloads/github-git-cheat-sheet.pdf)

# Python Cheet sheet

[Kaggle: Best cheat sheets for EDA, Data visualization, python libraries](https://www.kaggle.com/discussions/getting-started/254970)
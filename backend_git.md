-   [**Git**](https://git-scm.com/)

    -   **About**

        -   Git is a free and open source distributed version control
            system.

        -   **NOTE**: If you are using Cloud9, you do not have to
            install Git because it comes pre-installed. Otherwise,
            follow the
            [**Installation**](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
            instructions.

    -   **Basic Commands**

        -   **Init**

            -   To tell Git which directory it should monitor for
                changes, you must first cd \$DIR into the directory that
                you want to track, at which point Git will monitor that
                directory and all subdirectories upon executing the
                following code:\
                git init

            -   Upon execution, Git will create a **Hidden Folder**
                named **/.git** in your workspace. This hidden folder is
                where Git will track all changes to your workspace.

                -   **NOTE**: To view hidden folders in the terminal,
                    type: ls -a

                -   **ALSO**: If you accidentally git init in the wrong
                    directory, you can remove /.git like any other
                    directory: rm -rf .git

        -   **Status**

            -   Asks Git to provide a status report on your project. It
                will include information such as which **Branch** of the
                project you are on (e.g., master), which **Commit** you
                are on, whether any changes will be committed, whether
                any files have been modified since the last commit, and
                whether there are any **Untracked Files**.

        -   **Add**

            -   With respect to untracked files, Git does not
                automatically track every file within the directories
                monitored by init. Rather, a file must be specifically
                tracked by issuing the following command:\
                git add \$FILE

        -   **Commit**

            -   Once a file has been tracked, you can save the changes
                to a new version checkpoint in Git by running the
                following command:\
                git commit -m \"Explanatory Message (in Present Tense)\"

                -   **NOTE**: Every commit must include a message
                    explaining the changes that you are making. The -m
                    flag is used to signal the inclusion of a message.
                    If you do not include the -m flag with the message
                    in quotes, you will be prompted to enter a message
                    in the terminal.

                -   **IMPORTANT**: After you submit a commit for any
                    given file, Git will not automatically stage that
                    file for new commits in the future. Instead, you
                    must use add each time (i.e., a two-step process of
                    add and commit). If you want to add **[ALL
                    FILES]{.ul}** that have yet to be staged for a
                    commit, use:\
                    git add .

        -   **Log**

            -   Displays a log of all the commits made to the project:\
                git log

            -   For each commit, the log will include a long **Commit
                String**, which is the unique identifier for viewing
                each commit by using the following command.

        -   **Checkout**

            -   Primarily used for displaying a previous commit or may
                be used to change to a different
                [**Branch**](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell):\
                git checkout \$COMMIT\
                ...or...\
                git checkout -b \$BRANCH

                -   To return to the **Master Branch**, simply type:\
                    git checkout master

        -   **Revert**

            -   There are multiple ways to revert back to (rather than
                simply view) a previous commit (see, e.g., [How to
                revert Git repository to a previous
                commit?](https://stackoverflow.com/questions/4114095/how-to-revert-git-repository-to-a-previous-commit)),
                but one of the safest methods is as follows, which will
                revert everything back from the current HEAD to the
                specified commit string:\
                git revert \--no-commit \$COMMIT..HEAD\
                ...at which point you will then have to commit the
                reversion:\
                git commit -m \"Explanatory Message\"

            -   One of the main benefits to this method is that there
                are no actual deletions of any previous commits, and
                they can still be accessed through the log if desired.

    -   **Additional Resources**

        -   [Learn Git and GitHub Basics for
            Free](https://www.youtube.com/watch?v=LemSseuZB9I&list=PL86ehqHzxhy4XX_qZZE_5mrp38WGZRzTO)

        -   [Linking GitHub to
            Cloud9](http://lepidllama.net/blog/how-to-push-an-existing-cloud9-project-to-github/)


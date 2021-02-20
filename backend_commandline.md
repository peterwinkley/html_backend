-   **[The Command Line]{.ul}**

    -   **Resources**

        -   [**Getting to Know the Command
            Line**](https://www.davidbaumgold.com/tutorials/command-line/)

            -   **Finding the Command Line in Windows**

                -   There is no native command line in Windows. Install
                    [**Babun**](https://babun.github.io/).

            -   **Command Syntax**

                -   All commands have three parts: (1) the utility, (2)
                    the flags, and (3) the arguments. The utility always
                    comes first, and the other two are only needed
                    depending on the command that is used. Example:\
                    ls -l \~/Desktop\
                    (1) \"ls\" is a utility that is used to list the
                    contents of directories; (2) \"-l\" is a flag used
                    to indicate to the utility that we want more
                    information than usual, and so it should show the
                    directory in its \"long\" format; flags alter how
                    the utility operates; flags always start with either
                    one or two dashes, and they usually come between the
                    utility and arguments; (3) \"\~/Desktop\" is an
                    argument to the utility that tells it which
                    directory\'s contents to list; arguments are used
                    when the utility needs to know exactly what you want
                    for a certain action and there\'s no clear default
                    setting; arguments usually come at the end of the
                    command.

            -   **Basic Utilities**

                -   **Manual**: man \$UTIL

                    -   Get information for how to use any utility.
                        Replace \$UTIL with any utility, like ls, cd, or
                        even man. Press the up and down arrows to scroll
                        through the documentation. Press \"Q\" to quit
                        and go back to the command line.

                -   **List**: ls \$DIR

                    -   Lists the contents of the directory \$DIR. If no
                        directory is specified, lists the contents of
                        the current working directory. Use the -l flag
                        to get more information.

                -   **Change Directory**: cd \$DIR

                    -   Changes the current working directory to the
                        directory \$DIR. In effect, moves you around the
                        computer.

                -   **Print Working Directory**: pwd

                    -   If you ever get lost in the computer, run this
                        command to get a trail of breadcrumbs all the
                        way down from the top level of the computer to
                        see where you are.

                -   less \$FILE

                    -   Displays the contents of a file. Press the up
                        and down arrows to scroll though the file. Press
                        \"Q\" to quit and go back.

                -   **Copy**: cp \$FILE \$LOCATION

                    -   Copies the \$FILE to the \$LOCATION.

                -   **Move**: mv \$FILE \$LOCATION

                    -   Moves the \$FILE to the \$LOCATION.

                -   **Remove**: rm \$FILE

                    -   Deletes a file **permanently**: there is no way
                        to get it back.  Be careful when using this
                        command!

                    -   **NOTE**: Use the -rf (recursive force) flag to
                        remove an entire directory (including all
                        subdirectories and their contents).

                -   **Super User Do**: sudo \$CMD

                    -   When you use this utility, you use an entire
                        command as a single argument: for example, sudo
                        ls -l \~/Desktop.  Sudo asks for your user
                        account password. As a security measure, the
                        screen does not display anything as you type,
                        not even asterisks (\*). If the password is
                        typed in correctly, sudo executes the \$CMD with
                        elevated permissions.  Be careful when using
                        this command!

        -   [**Survival Guide for Unix
            Newbies**](http://matt.might.net/articles/basic-unix/)

        -   [**Learn Enough Command Line to Be
            Dangerous**](https://www.learnenough.com/command-line-tutorial)

    -   **Other Command Line Utilities**

        -   **Create File**: touch \$FILE.ext

            -   Creates a new file.

        -   **Make Directory**: mkdir \$DIR

            -   Creates a new folder.


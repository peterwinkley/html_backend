-   [**Node.js**](https://nodejs.org/en/)

    -   **About**

        -   Previously, all JS had to run in the browser (which means it
            was all front-end code and not server-side). However,
            Node.js now allows JS to be run server-side.

    -   **Usage**

        -   The **Node Console** is purely server-side and therefore has
            no HTML/CSS to interact with. Rather, all JS is executed in
            the command line via the terminal.

            -   **NOTE**: Not all JS features are available in the Node
                console (e.g., \"alert\", \"document\", or any DOM
                manipulation), because such functions come with the
                browser.

        -   To **Enter** the Node console from the terminal in Cloud9,
            type \"node\". To **Exit**, hit \"CTRL + C\".

        -   For the most part, you will not be running code out of the
            Node console; rather, a more common task is to run JS files
            via Node by typing \"node fileName.js\".

    -   [**NPM (Node Package Manager)**](https://www.npmjs.com/)

        -   **About**

            -   To use libraries (e.g., jQuery or Bootstrap\'s JS
                library) on the front end, you would use a \<script\>
                tag to include the library. However, there is no HTML on
                the server-side to perform these functions. NPM allows
                you to include those libraries (a.k.a. \"packages\") in
                a repository on the NPM website, and which can be
                installed via its command line tool.

        -   **Installing NPM Packages**

            -   Use \"npm install packageName\" to **Install** a
                package, e.g.:\
                npm install cat-me

            -   Upon installation, a new directory entitled
                \"node_modules\" should appear that contains the
                specified package (and any subsequent packages that are
                installed will appear inside the same directory).

            -   After the installation, you must then **Import** the
                package into your application via the \"require\"
                command in order to use the package:\
                var catMe = require(\'cat-me\');

                -   **NOTE**: \"catMe\" can be any name you want it to
                    be.

            -   Once the package has been imported into a variable,
                refer to the NPM\'s packages documentation to learn how
                to use the package.

        -   **Package.json**

            -   **About**: All npm packages contain a file (usually in
                the project root) called package.json (JSON: JavaScript
                Object Notation), which holds various metadata relevant
                to the project (re: identification, dependencies,
                description, version, license, configuration). Node
                itself is only aware of two fields in the package.json
                file: name and version.

                -   **NOTE**: Dependencies specified in package.json
                    will be installed to a folder called
                    **node_modules** upon package installation.

            -   Use \"npm init\" to **Create** a new package.json file.

            -   When you use the \"\--save\" flag in tandem with \"npm
                install\", it will take the package name and version and
                automatically save it into your **Dependencies** in your
                package.json file, if you have one.

                -   **IMPORTANT**: When naming your project, be sure
                    that the project is not given the same name as that
                    of any NPM library that your project will use as a
                    dependency. If you do, you will receive an error
                    upon attempting to install the library as a
                    dependency. To correct this problem, you will have
                    to access the package.json file and modify the
                    \"name\" property.


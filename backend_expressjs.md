-   [**Express.js**](https://expressjs.com/)

    -   **About**

        -   Express is a web development **Framework**, which is like a
            library/package in the sense that a framework is a set of
            external code included in your application. However,
            libraries differ in the sense that you are in full control
            (e.g., you can pick and choose which methods you wish to
            use), whereas you give up much more control when using a
            framework (i.e., the framework gives you the
            \"scaffolding\", and you get to fill it in).

        -   **NOTE**: Other web development frameworks include Flask,
            Django, Rails, etc. Express is known as a \"light-weight\"
            framework in which there is more \"white space\" for you to
            fill in yourself, as opposed to a \"heavy-weight\" framework
            like Rails, in which most of the work has already been done
            for you.

    -   **Installation**

        -   Use \"npm install express\" in the Cloud9 terminal, and then
            \"require\" it in your application, e.g.:\
            var express = require(\'express\');

            -   **NOTE**: Unlike \"cat-me\", Express cannot simply be
                run by simply saying, e.g., express(). This is because,
                unlike cat-me, Express is not just one simple function
                but rather a collection of many different methods.
                Therefore, the best approach is to save its execution as
                a variable (conventionally named \"app\") to which
                methods can later be appended (app.whatever) as needed,
                e.g.:\
                var app = express();

        -   When working outside of the Cloud9 terminal, use the
            \"**\--**save\" flag to include Express in your package.json
            dependencies:\
            \"npm install express \--save\"

    -   **Routing**

        -   **Basics**

            -   Express relies on routing to determine how an
                application responds to a client request to a particular
                endpoint, which is a [**Uniform Resource
                Identifier**](https://developer.mozilla.org/en-US/docs/Glossary/URI)
                (or path) and a specific HTTP request method (e.g., GET,
                POST, etc.). Each route can have one or more handler
                functions, which are executed when the route is matched.

            -   Routes follow the syntax below:\
                app.\$METHOD(\$PATH, \$HANDLER)

                -   \(1\) app is an instance of Express;

                -   \(2\) METHOD is an HTTP request method, in
                    lowercase;

                -   \(3\) PATH is a path on the server; and

                -   \(4\) HANDLER is the function executed when the
                    route is matched.

            -   Specifically, a route could provide a message when a
                user accesses the home page:\
                app.get(\'/\', function(req, res) {\
                res.send(\'hello\');\
                });

                -   **NOTE**: \"req\" (**Request**) and \"res\"
                    (**Response**) are actually objects (and can be
                    named whatever you want, although req and res are
                    conventions). \"Request\" is an object containing
                    all the information about the request that was made
                    and which triggered the route. \"Response\" is an
                    object containing all of the data that the server is
                    going to respond with.

        -   **Parameters**

            -   The **Splat (\*)** parameter acts as a wildcard, and it
                will trigger whenever the user attempts to access an
                undefined or unexpected route, e.g.:\
                app.get(\'\*\', function(req, res) {\
                res.send(\'Error. Page not found.\');\
                });

                -   **NOTE**: The splat parameter should appear at the
                    **END** of your routes, as it will supersede all
                    other subsequent matching routes (the first route
                    that matches a given request is the only route that
                    will be run). The order of routes always matters
                    (like the order of execution in a series of if/else
                    if conditions).

            -   **Route Parameters** (a.k.a., route variables or path
                variables) are used to define and identify patterns in a
                route by using a colon in front of anything that you
                want to be a variable. For example, in the case of
                Reddit, the following pattern is used:\
                app.get(\'/r/:subredditName\', function(req, res) {\
                var subreddit = req.params.subredditName;\
                res.send(\'Welcome to the \' + subreddit + \'
                subreddit.\');\
                });\
                ...or...\
                app.get(\'/r/:subredditName/comments/:postId/:postTitle/\',
                function(req, res) {\
                res.send(\'\<h1\>Welcome to the comments
                page.\</h1\>\');\
                });

                -   **NOTE**: In order to know what is the value of
                    \":subredditName\", it is possible to access that
                    data by entering the \"req\" object, which contains
                    all of the information about the incoming request.
                    The simplest way to access that data is to use
                    console.log(req), and then search for the value
                    listed in \"**params**\" -- or simply use
                    console.log(req.params) to see all route parameters.

                -   **ALSO NOTE**: HTML tags can be included in the sent
                    response.

    -   **Listening**

        -   In order for Express to know when to serve a response to a
            request, it must be told to **Listen** for requests. This is
            done by using \"app.listen()\" together with the port number
            that should be listened to, e.g.:\
            app.listen(3000);

            -   **NOTE**: On Cloud9, you have to use the website\'s own
                assigned port and IP address, which can vary. Therefore,
                use the following syntax to obtain the variable
                port/IP:\
                app.listen(process.env.PORT, process.env.IP);

    -   **Templates and EJS**

        -   **Basics**

            -   As noted above, HTML elements can be used in sending a
                response; however, transmitting an entire HTML page in
                this manner would be cumbersome. Instead, you can use a
                method called **Render** (as appended to the \"res\"
                object), e.g.:\
                app.get(\'/\', function(req, res) {\
                res.render(\'home.html\');\
                });

            -   However, when working with Express, you typically will
                not write plain HTML files (standard static files);
                rather, you will likely be using dynamic HTML files that
                are called **Templates**, which exist as **EJS**
                (Embedded JavaScript) files, and **[WHICH MUST BE
                LOCATED IN A DIRECTORY CALLED \"VIEWS\"]{.ul}** to be
                rendered by Express, e.g.:\
                app.get(\'/\', function(req, res) {\
                res.render(\'home.ejs\');\
                });

                -   **NOTE**: Using EJS files requires installing EJS:
                    npm install ejs \--save

                -   **ALSO NOTE**: Express\'s special handling of the
                    \"views\" directory also includes the option to
                    specify ahead of time what type of files will be
                    served from \"views\". Thus, rather than needing to
                    specify res.render(\'love.ejs\'), you can simply say
                    res.render(\'love\'). To do so, simply include the
                    following line if your instance of Express:\
                    **app.set(\'view engine\', \'ejs\');**

            -   To understand the **Syntax** of EJS, refer to the [**EJS
                Documentation**](https://www.npmjs.com/package/ejs). But
                it is important to note that variables set in the GET
                callback function do not automatically pass through to
                the variable names used in the EJS file. Rather, such
                information must be passed through the res.render()
                method as an object (which can contain multiple pieces
                of data that you want available in your template),
                e.g.:\
                app.get(\'love/:thing\', function(req, res) {\
                var thing = req.params.thing;\
                res.render(\'love.ejs\', {thingVar: thing});\
                });\
                ...then in the love.ejs file, you can use the following
                **EJS Tags**:\
                I love \<%= thingVar %\>!\
                ...and the end result for a route such as \"love/cats\"
                will be, \"I love cats!\"

                -   **NOTE**: If you want your EJS to execute **HTML**
                    tags rather than just display them if contained in a
                    given variable, then use \<%- instead. However,
                    caution should be exercised, as \<script\> tags can
                    be executed in HTML. This problem can be remedied by
                    **Sanitizing** your inputs. One popular package is
                    [**Express
                    Sanitizer**](https://www.npmjs.com/package/express-sanitizer).

                    -   To install Express Sanitizer, run:\
                        npm install express-sanitizer \--save\
                        ...require it:\
                        var expressSanitizer =
                        require(\'express-sanitizer\');\
                        ...and place the following **AFTER** **YOUR BODY
                        PARSER** (explained below in the \"POST
                        Requests\" section):\
                        app.use(expressSanitizer());\
                        ...then place the following in your create and
                        update routes prior to running the actual create
                        and update methods:\
                        req.body.propertyToSanitize =
                        req.sanitize(req.body.propertyToSanitize);

        -   **Conditionals**

            -   To include conditional if/else logic in an EJS file,
                wrap each individual line of JS in the following EJS
                tags, e.g.:\
                \<% if (thingVar === \'dogs\') { %\>\
                \<p\>Good choice! Dogs are the best!\</p\>\
                \<% } else { %\>\
                \<p\>Bad choice! You should love dogs!\</p\>\
                \<% } %\>

                -   **NOTE**: EJS tags with the **Equals Sign** are used
                    to take the returned value and render it to the page
                    as HTML. On the other hand, if you are simply
                    executing logic (conditionals/loops) that should not
                    be displayed in the HTML, then use EJS tags without
                    the equals sign.

        -   **Loops**

            -   Loops operate in the same manner as described above.
                Create your set of data in your instance of Express to
                be passed to the EJS file, e.g.:\
                app.get(\'/posts\', function(req, res) {

> var posts = \[
>
> {title: \'My dog has fleas.\', author: \'Alex\'},
>
> {title: \'All cows eat grass.\', author: \'Bob\'},
>
> {title: \'Every good boy deserves fudge.\', author: \'Charlie\'},
>
> \];
>
> res.render(\'posts.ejs\', {postsVar: posts});
>
> });\
> ...and then in the posts.ejs file:\
> \<h1\>Posts\</h1\>
>
> \<% postsVar.forEach(function(post) { %\>
>
> \<p\>\<strong\>Title:\</strong\> \<%= post.title %\>
>
> \<strong\>Author:\</strong\> \<%= post.author %\> \</p\>
>
> \<% }); %\>

-   **Serving Custom Assets (CSS/JS)**

    -   To serve a response that includes more than just plain HTML,
        Express requires that custom assets (such as CSS and JS files)
        be configured in a particular manner. Convention calls for
        giving your workspace\'s asset folder the name \"**Public**\".
        If you are working with a CSS file, you would then create a new
        filed named, for example, \"style.css\" in \"public\". To link
        to this resource, you need not specify both the directory and
        file name. Rather, you should just use the file name in the
        link, and then include the following line in your instance of
        Express to tell it where to find custom assets:\
        **app.use(express.static(\'public\'));**

        -   **NOTE**: The default behavior of Express is to not serve
            anything aside from the \"views\" directory, unless it is
            specifically instructed to do so. This is why it is
            necessary to tell Express explicitly to serve the \"public\"
            directory, although not required for the \"views\"
            directory.

        -   **ALSO NOTE**: Out of the abundance of caution, you can
            state the following as an alternative:\
            app.use(express.static(**\_\_dirname +** \'/public\'));\
            By incorporating \"**dirname**\", you are specifically
            instructing Node to look for \"public\" in the same
            directory name that the app.js file is saved within. But
            note that, when doing so, you must add the **Backslash**
            before \"public\".

    -   However, since each page will require standard HTML boilerplate
        (in which links to custom assets will be included), it is
        necessary to create a template for your HTML \"header\"
        (everything above and including the opening \<body\> tag) and
        \"footer\" (everything to appear at the end of all pages on the
        website until the closing \</html\> tag). Such headers and
        footers are called **Partials**, and they are typically named
        \"header.ejs\" and \"footer.ejs\" respectively, and they are
        stored in the \"**views/partials**\" directory. Once created,
        you can simply include the following at the top and bottom of
        each of your EJS files:\
        \<% include partials/header %\>\
        ...\
        \<% include partials/footer %\>

        -   **IMPORTANT**: When linking to an asset (such as
            \"style.css\" in \"public\"), even though Express knows to
            look in the \"public\" directory to find the asset, it will
            only be recognized to exist in your workspace\'s root
            directory (e.g.,
            [www.website.com/](http://www.website.com/)), and not
            whatever subdirectory the user may currently be in (e.g.,
            [www.website.com/info/](http://www.website.com/info/)).
            Therefore, if you want all pages on your site to share a
            common CSS file, it is **[CRITICAL]{.ul}** to ensure that
            all stylesheet links in your HTML header state that the
            resource originates in the **[ROOT DIRECTORY]{.ul}**, i.e.:\
            \<link rel=\"stylesheet\" type=\"text/css\"
            href=\"**/**style.css\"\>\
            ...**[NOT]{.ul}**...\
            \<link rel=\"stylesheet\" type=\"text/css\"
            href=\"style.css\"\>

            -   **NOTE**: If you were to put your CSS file into a
                subdirectory called \"stylesheets\" in your \"public\"
                directory, then you would say,
                href=\"/stylesheets/style.css\". The main thing is to
                include the root \"/\" character.

        -   **NOTE**: Depending on the location of your workspace, it
            may be necessary to say **./partials/header** rather than
            just partials/header. The **Dot-Slash** is necessary should
            you want Node to use whatever your **CURRENT DIRECTORY** is
            as its point of reference, as opposed to the root directory
            (i.e., tell Node to find the header file in the partials
            directory of whatever directory you\'re currently in, not
            the header file in the partials directory of the root
            directory).

            -   Should it be necessary to move **BACK ONE DIRECTORY**,
                then instead use **Dot-Dot-Slash**:
                **../partials/header**. You can use the dot-dot-slash as
                many times as necessary to move back as many directories
                as you need to, e.g.: ../../partials/header.

-   **POST Requests**

    -   To create a new POST request, you must use the **app.post()**
        method in your Express instance, and route the request to a
        particular location, e.g.:\
        app.post(\'/friends\', function(req, res) {

> // insert code here
>
> });\
> ....and then use that same location in, for example, the HTML form in
> the EJS file that will be used to transmit the POST request:\
> \<form action=\"/friends\" method=\"POST\"\>
>
> \<input type=\"text\" name=\"newfriend\" placeholder=\"New Friend\"\>
>
> \<button\>Add!\</button\>
>
> \</form\>

-   **NOTE**: The input must be given a name, as that will be the
    \"key\" by which we can look it up inside of the route. When the
    request is sent, the value of the single \"newfriend\" property will
    be sent in the body of the request. To see this illustrated, you can
    use console.log(req.body), in which \"req.body\" is an object that
    contains all of the data from the request body. **HOWEVER**, Express
    lacks the ability to create \"req.body\" by default; therefore, it
    is necessary to use \"**npm install body-parser \--save**\", as this
    will take the request body and turn it into a JS object that can be
    used. [**Body Parser**](https://www.npmjs.com/package/body-parser)
    can be used when there is a form from which you would like to
    extract data on the server side.

    -   **ALSO**: The following lines must be used for the body parser
        to work:\
        **var bodyParser = require(\'body-parser\');\
        app.use(bodyParser.urlencoded({ extended: true }));**

        -   **NOTE**: Refer to the documentation for further
            explanation.

```{=html}
<!-- -->
```
-   Upon the form\'s submission, the following can be run to (1) create
    a new variable containing the desired value from req.body, (2) push
    the value to an array of the list of friends, and (3) **Redirect**
    the user back to the friends list / submission page:\
    app.post(\'/friends\', function(req, res) {

> var newFriend = req.body.newfriend;
>
> friendsList.push(newFriend);
>
> res.redirect(\'friends.ejs\');\
> });

-   **NOTE**: See the section below on RESTful routing for additional
    information on naming conventions.

-   **ALSO**: If you want to redirect a user **Back** to the **HTTP
    Referer**, use:\
    res.redirect(\'back\');

```{=html}
<!-- -->
```
-   **IMPORTANT**: Because the request body will be returned as an
    object, that means you can take advantage of [**BRACKET NOTATION**
    **WHEN NAMING YOUR INPUTS**]{.ul}. For example, if you have a form
    that will submit a new blog post with three properties (title,
    image, and body), you can set the name of each input as follows:
    name=\"blog\[title\]\", name=\"blog\[image\]\", and
    name=\"blog\[body\]\". The result will be the compilation of a
    single \"blog\" object with three properties for title, image, and
    body (with each having its value defined by the user). When used
    together with Body Parser, the entire object can be accessed by
    simply stating: req.body.blog

```{=html}
<!-- -->
```
-   **PUT Requests**

    -   As a preliminary matter, making a PUT request will require
        installing
        [**Method-Override**](https://www.npmjs.com/package/method-override),
        which allows you to use HTTP verbs such as PUT or DELETE in
        places where the browser does not support such requests by
        default. Most notably, **HTML Forms** were only designed to
        recognize GET and POST requests. Thus, editing database content
        via a form element will require the installation and use of
        method-override as follows:\
        npm install method-override \--save\
        ...together with:\
        var methodOverride = require(\'method-override\');\
        app.use(methodOverride(\'\_method\'));

        -   **NOTE**: The underscore in \'\_method\' is known as a
            \"**Getter**,\" which is used to look up the overridden
            request. The \'\_method\' getter is used to override the
            method via a query string, which is detailed further in the
            next bullet point. If you fail to implement these steps,
            your PUT request will default to a GET request.

    -   A PUT request is used to replace the data contained in a given
        resource. To create a new PUT request, you must use the
        **app.put()** method and route the request, e.g.:\
        app.put(\'/friends/:id\', function(req, res) {\
        // insert code here\
        });\
        ...and then use that same location in the HTML form in the EJS
        file that will be used to transmit the PUT request to a specific
        database object based upon its ID:\
        \<form action=\"/friends/\<%= friend.id %\>?\_method=PUT\"
        method=\"POST\"\>

> \<input type=\"text\" name=\"friend\[name\]\" value=\"\<%= friend.name
> %\>\"\>
>
> \<button\>Update!\</button\>
>
> \</form\>

-   **NOTE**: When using method-override, you have to set the \"normal\"
    method to POST. Once you set the value of the \"\_method\" query
    string to \"PUT\", the subsequent method=\"POST\" is overridden to
    be a true PUT request.

```{=html}
<!-- -->
```
-   **DELETE Requests**

    -   DELETE requests are essentially handled in the same manner as
        PUT requests, e.g.:\
        app.delete(\'/friends/:id\', function(req, res) {\
        // insert code here\
        });\
        ...although they differ insofar as the only data that needs to
        be passed through is the parameter associated with the data
        object to be deleted (which, in this case, would be the ID
        tag):\
        \<form action=\"/friends/\<%= friend.id %\>?\_method=DELETE\"
        method=\"POST\"\>

> \<button\>Delete!\</button\>
>
> \</form\>

-   **Refactoring Routes**

    -   Rather than having all of your routes in your app.js file, you
        can break them up by functionality across multiple JS files,
        which can then be required by the app.js file. Convention calls
        for placing each file in a directory named \"routes\". Each
        route file can be given a specific name, although an
        \"all-purpose\" routes file is typically named \"index.js\".

    -   After your routes have been divided into separate JS files, you
        can use the **Express Router** by placing the following code at
        the top of each file:\
        var express = require(\'express\');\
        var router = express.Router();\
        ... then (1) substitute \"app\" for \"router\" in each route,
        and (2) export the output at the end of the file by using the
        following code:\
        module.exports = router();

    -   After your JS files have been configured and all required
        dependencies have been included, the exported data can then be
        assigned to a variable in app.js, which can be used by the
        application, e.g.:\
        var friendRoutes = require(\'./routes/friend\');\
        ...\
        app.use(friendRoutes);

        -   **NOTE**: If you want to further \"dry\" up your code, you
            can specify in the \"app.use()\" line what particular
            language should appear in each route, and then you can
            append any additional required language in the route\'s JS
            file. For example, if your \"friends\" routes all start with
            \"/friends\" (e.g., \"/friends/new\", \"/friends/:id\",
            \"/friends/:id/edit\"), you can simplify your code by
            stating the following in your app.js file:\
            app.use(\'/friends\', friendRoutes);\
            ... and then remove \'/friends\' from the beginning of all
            routes in your friend.js routes file, as they will no longer
            be necessary (all that is needed is to include \"/new\",
            \"/:id\", \"/:id/edit\", and so forth).

        -   **IMPORTANT**: To pass **Parameters** (such as \":id\"
            above) from the app.js file to the route\'s JS file, it is
            necessary to include the following in your router variable:\
            var router = express.Router(**{ mergeParams: true }**);

```{=html}
<!-- -->
```
-   [**RESTful (\"Representational State Transfer\")** **Routing**]{.ul}

-   **[Authentication]{.ul}**

    -   [**Passport.js**](http://www.passportjs.org/)

        -   Passport.js provides a simple user authentication process
            for all Express-based applications. Passport includes over
            300 types of \"**Strategies**\", which are methods of
            authentication that include, *inter alia*, \"local\"
            authentication (i.e., traditional e-mail/password logins) or
            authentication via services such as Twitter, Google, etc.

        -   One popular **Local** Passport strategy for simple
            **Username/Password** authentication is
            [**Passport-Local**](https://github.com/jaredhanson/passport-local).
            If using Mongoose, you can implement such authentication
            even faster by also adding
            [**Passport-Local-Mongoose**](https://github.com/saintedlama/passport-local-mongoose).

    -   **Sessions**

        -   Authentication is made possible through \"sessions.\" HTTP
            is supposed to be a stateless protocol, which means that
            when a request is made, it does not contain any information
            about your history of activity on the site (i.e., a request
            does not have a \"state\"; it is a one-time transaction).
            However, a session applies a persistent \"state\" to a
            user\'s HTTP requests when that user is logged in to the
            server, thereby allowing encrypted information about the
            user to be saved to each HTTP request. Passport will then
            translate that encrypted login information when configuring
            its HTTP response.

        -   [**Express-Session**](https://www.npmjs.com/package/express-session)
            is an npm package that facilitates session creation in
            Express apps.

    -   **Node/Express/Mongoose Setup**

        -   **Installation**

            -   Apply the standard npm installation commands for (1)
                Body-Parser, (2) EJS, (3) Express, (4)
                Express-Session, (5) Mongoose, (6) Passport, (7)
                Passport-Local, and (8) Passport-Local-Mongoose:\
                npm install express body-parser ejs express-session
                mongoose passport passport-local passport-local-mongoose
                \--save

        -   **Configuration**

            1.  Set the standard \"require\" variables, but note that
                > convention calls for naming the Passport-Local
                > variable \"**LocalStrategy**\", and the
                > Express-Session variable \"**session**\":\
                > var express = require(\'express\'),

> session = require(\'express-session\'),
>
> bodyParser = require(\'body-parser\'),
>
> mongoose = require(\'mongoose\'),
>
> passport = require(\'passport\'),
>
> LocalStrategy = require(\'passport-local\'),
>
> passportLocalMongoose = require(\'passport-local-mongoose\'),
>
> app = express();

-   **NOTE**: Generally, variables start with a lowercase letter.
    However, convention calls for capitalizing **Constructors**, which
    are \"blueprints\" for creating many objects of the same \"type.\"
    In this case, LocalStrategy is one such constructor and thus begins
    with an uppercase letter. You can typically know that something is a
    constructor if it is used in tandem with the
    \"[**new**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new)\"
    operator to create an object (e.g., **new** mongoose.**Schema**...).

(1) Instruct Express to use Express-Session, e.g.:\
    app.use(session({\
    secret: \'whatever you want\';\
    resave: false,\
    saveUninitialized: false\
    }));

    -   **NOTE**: The **Secret** phrase is used to encode and decode the
        session; thereby allowing encrypted data to be stored during the
        session, rather than storing the username and password in plain
        text. Refer to the [**Express-Session
        Documentation**](https://www.npmjs.com/package/express-session)
        for more information on \"resave\" and \"saveUninitialized\".
        However, for basic usage purposes, simply know that those
        properties will default to \"true\" if left unspecified, and you
        will generally want those changed to \"false\".

(2) Instruct Express to use Passport by entering the following commands
    in app.js:\
    app.use(passport.initialize());\
    app.use(passport.session());

    -   **NOTE**: The second line is required for applications that use
        **Persistent Login Sessions** (which is recommended). This
        \"session\" actually refers to a strategy that is bundled with
        Passport (**NOT** the variable \"session\" for
        \"express-session\").

    -   **IMPORTANT**: These two lines of code must appear **AFTER THE
        EXPRESS-SESSION** code in step 2 above. Otherwise,
        authentication will not work.

(3) Configure Mongoose, create your model file(s) (e.g.,
    models/user.js), and add Passport to your model(s) to be exported
    back to your app, e.g.:\
    var mongoose = require(\'mongoose\');

> var passportLocalMongoose = require(\'passport-local-mongoose\');
>
> var userSchema = new mongoose.Schema({\
> username: String,\
> password: String\
> });
>
> userSchema.plugin(passportLocalMongoose);
>
> module.exports = mongoose.model(\'User\', userSchema);

-   **NOTE**: The fourth line adds methods from the
    passport-local-mongoose package to the User schema, and it works in
    tandem with step 5 below.

(4) Create a User variable that requires the exported data from step 4
    above, and instruct your app use Passport\'s **Serialization**
    commands to encode and decode each User session:\
    var User = require(\'./models/user\');\
    passport.use(new LocalStrategy(User.authenticate()));\
    passport.serializeUser(User.serializeUser());\
    passport.deserializeUser(User.deserializeUser());

-   **NOTE**: The second line creates a new LocalStrategy (via
    passport-local) by using the authenticate() method that was imported
    from the User model (via passport-local-mongoose).

-   **ALSO NOTE**: serializeUser() and deserializeUser() are methods
    added from passport-local-mongoose via the plugin method in step 4
    above.

    -   **Routing**

        -   **Registration**

            -   After creating a registration form (with, for example, a
                post request to \"/register\" and with two inputs named
                \"username\" and \"password\") for accessing a
                \"secret\" page, create the form\'s **POST Route** as
                follows:\
                app.post(\'/register\', function(req, res) {\
                User.register(new User({ username: req.body.username
                }),\
                req.body.password, function(err, user) {\
                if (err) {\
                console.log(err);\
                return res.redirect(\'/register\');\
                }\
                passport.authenticate(\'local\')(req, res, function() {\
                res.redirect(\'/secret\');\
                });\
                });\
                });

                -   **IMPORTANT**: The **register()** method is used to
                    create the username and password according to the
                    User model. However, it is important to note that
                    the password entered by the user is **NOT SAVED TO
                    THE USER MODEL**; rather, it is passed as the second
                    argument in the register method. This allows the
                    password to be encoded and saved to the User model
                    in an encoded format (by means of [**Salted Password
                    Hashing**](https://en.wikipedia.org/wiki/Salt_(cryptography))).

                -   **ALSO NOTE**: The **authenticate()** method is used
                    to specify which Passport strategy you are using (in
                    this case, \"local\").

        -   **Login**

            -   After creating a login form (with, for example, a post
                request to \"/login\" and with two inputs named
                \"username\" and \"password\") for accessing a
                \"secret\" page, create the form\'s **POST Route** as
                follows:\
                app.post(\'/login\', passport.authenticate(\'local\', {\
                successRedirect: \'/secret\',\
                failureRedirect: \'/login\'\
                }), function(req, res) {});

                -   **NOTE**: The **authenticate()** method is being
                    used here as **Middleware**, which is the name given
                    to any code that runs immediately after the route is
                    accessed but before the final callback at the end.
                    This particular piece of middleware is set up (at
                    least in part) by the second line in step 5 of the
                    configuration section above. In this case,
                    authenticate() attempts to log the user in. If the
                    attempt is successful, then the user will be
                    redirected to the \"secret\" page. If the attempt
                    fails, then the user will be redirected back to the
                    \"login\" page.

                -   **ALSO NOTE**: Technically, the final callback at
                    the end is not required, but it should be retained
                    to remind you that the authenticate() method is
                    middleware.

        -   **Logout**

            -   After creating a logout link that directs to
                \"/logout\", create a **GET Route** as follows:\
                app.get(\'/logout\', function(req, res) {\
                req.logout();\
                res.redirect(\'/\');\
                });

    -   **Authentication (Middleware)**

        -   To stop someone from accessing a \"secret\" page if they are
            not logged in, it is necessary to create your own
            **Middleware Function** to the \"secret\" route. For
            example, if the user is logged in, then the route will
            render the \"secret\" page; otherwise, the user will be
            directed to the index page:\
            function isLoggedIn(req, res, next) {\
            if (req.isAuthenticated()) {\
            return next();\
            }\
            res.redirect(\'/login\');\
            }\
            ...which can then be added as middleware to the \"secret\"
            route as follows:\
            app.get(\'/secret\', isLoggedIn, function(req, res) {\
            res.render(\'secret\');\
            });

            -   **NOTE**: The three arguments for the \"isLoggedIn\"
                function are standard arguments that are recognized by
                Express. The \"next\" argument is understood by Express
                to require that the code following the middleware be
                executed if the isAuthenticated() method returns true.

        -   If you want to pass through information about the user to be
            manipulated as a JS object in your rendered templates, this
            can be done by accessing \"**req.user**\", which contains
            all of the information about the logged-in user who is
            submitting a particular request, e.g.:

> app.get(\'/\', function(req, res) {
>
> res.render(\'index\', { currentUser: req.user });
>
> });\
> ...which then allows you to customize page display depending on
> whether or not a user is logged in (e.g., display login/registration
> links if a user is not signed in, but otherwise display the user\'s
> name and a logout link if a user is signed in):\
> \<% if (!currentUser) { %\>
>
> \<a href=\"/login\"\>Login\</a\>
>
> \<a href=\"/register\"\>Sign Up\</a\>
>
> \<% } else { %\>
>
> \<span\>Welcome, \<%= currentUser.username %\>\</span\>
>
> \<a href=\"/logout\"\>Logout\</a\>
>
> \<% } %\>

-   **IMPORTANT**: If you want to have access to req.user on **EVERY
    ROUTE** without having to manually pass through an object each time
    a page is rendered, then simply use the following **Middleware
    Function** (to be run on every Express route):\
    app.use(function(req, res, next) {

> res.locals.currentUser = req.user;\
> next();
>
> });

-   Essentially, whatever value is assigned to \"res.locals.object\"
    will be made available inside of every template.

-   **NOTE**: This function must go **AFTER** the code listed in step 3
    of the configuration section above.

```{=html}
<!-- -->
```
-   **ALSO IMPORTANT**: If you are performing **Authorization** (i.e.,
    only a user who created a comment has the authority to edit it),
    then you will have to compare req.user.\_id with the ID of the
    creator of the comment. However, the comment creator\'s ID
    (generated pursuant to a schema) is stored as a Mongoose **OBJECT**,
    whereas req.user.\_id is stored as a **STRING**. Therefore, it is
    necessary to convert the former to a string to make a comparison
    (via the **String()** function). Alternatively, Mongoose comes with
    a built-in **Equals** method that performs this task, e.g.:\
    if (foundComment.author.id.equals(req.user.\_id)) {\
    // insert code here\
    }

```{=html}
<!-- -->
```
-   **NOTE**: For more information about authentication in Node, refer
    to the following presentations by Randall Degges:

    -   [Everything You Ever Wanted to Know about Node
        Authentication (2014)](https://www.youtube.com/watch?v=FkPqcIJvEPk)

    -   [Everything You Ever Wanted to Know About Web Authentication in
        Node (2017)](https://www.youtube.com/watch?v=i7of02icPyQ)

```{=html}
<!-- -->
```
-   **[Refactoring Middleware]{.ul}**

    -   Rather than keeping middleware in separate files, you can
        combine them into a single JS file. In that file, you can create
        a **Middleware Object** to which you can append all of the
        middleware functions as **Methods**. (In JS, every function is
        an object. An object is a collection of key:value pairs. When
        the value is a primitive (i.e., integer, string, or Boolean) or
        another object, the value is called a \"property.\" When the
        value is a function, it is called a \"method.\") Example:\
        var middleware = {\
        firstMiddleware: function() {\
        // do something\
        },\
        secondMiddleware: function() {\
        // do something\
        }\
        }\
        module.exports = middleware;

        -   **NOTE:** Alternatively, you may create an empty middleware
            object (i.e., var middleware = {};) and then separately add
            each method to the middleware object, e.g.:\
            middleware.firstMiddleware = function() {\
            // do something\
            };

    -   When using Node, convention calls for naming the compiled
        middleware file **Index.js**, and saving it to an appropriately
        titled directory (e.g., \"middleware\"). This is because Node
        always uses the file named index.js by **Default** when a
        directory has been required. Thus, the \"require\" code need
        only state the directory path rather than the directory and
        specific file name, e.g.:\
        var middleware = require(\'../middleware\');

-   **[Flash Messages]{.ul}**

    -   [**Connect-Flash**](https://www.npmjs.com/package/connect-flash):

        -   Connect-flash uses the **Flash**, which is a special area of
            the session used for storing messages. Messages are written
            to the flash and cleared after being displayed to the user.
            The flash is typically used in combination with redirects,
            ensuring that the message is available to the next page that
            is to be rendered.

    -   **Installation & Configuration**:

        -   Apply the standard npm command, require the package, and use
            it (**PRIOR TO** Passport):\
            npm install connect-flash \--save\
            var flash = require(\'connect-flash\');\
            app.use(flash());

        -   **NOTE**: The installation instructions on the npm page also
            include references to \"cookieParser\" and \"session\".
            However, this inclusion is not necessary if you are already
            using express-session.

    -   **Usage**:

        -   When you want to display a message, use the following syntax
            (following a key-value pair format) **BEFORE** redirecting
            to the page in which the message will be displayed:\
            req.flash(\'keyName\', \'valueMessage\');\
            res.redirect(\'/pageName\');

        -   For example, if you want to have an error message be
            displayed to an unauthenticated user and then redirect the
            user to the login page, you could add the following to your
            authentication middleware...\
            req.flash(\'error\', \'Please login first.\');\
            res.redirect(\'/login\');\
            ...and then you must **Handle** the flash message in the
            actual login route...\
            router.get(\'/login\', function(req, res) {\
            res.render(\'login\', { **message: req.flash(\'error\')**
            });\
            });\
            ...and then add the message to the appropriate login.ejs
            **Template**, e.g.:\
            \<div\>\<%= message %\>\</div\>

            -   **IMPORTANT**: Per the connect-flash docs, you may
                either set a flash message on the req.flash object
                before returning res.**redirect** **[OR]{.ul}** you may
                pass the req.flash object into the res.render function;
                **[HOWEVER]{.ul}**, it will not work if you attempt to
                set a flash message on the req.flash object before
                returning res.**render**.

            -   **NOTE**: If you want to have access to req.flash on
                **EVERY ROUTE** without having to manually pass through
                an object each time a page is rendered, then simply use
                the following **Middleware Function** that is also used
                for req.user as noted in the \"Authentication\" section
                above:\
                app.use(function(req, res, next) {

> res.locals.currentUser = req.user;\
> **res.locals.message = req.flash(\'error\');**\
> next();\
> });

-   **ALSO NOTE**: The \"message\" need not be named as such, and can be
    given any name to distinguish it from **Multiple** flash messages,
    e.g.:\
    app.use(function(req, res, next) {

> res.locals.currentUser = req.user;\
> res.locals.error = req.flash(\'error\');\
> res.locals.success = req.flash(\'success\');\
> next();\
> });


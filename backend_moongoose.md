    ```
    -   [**Mongoose**](http://mongoosejs.com/)

        -   **About**

            -   Mongoose is an NPM package that helps you interact with
                MongoDB inside of your JS files. Mongoose is known as an
                **Object-Document Mapper** (ODM), which basically means
                it is a way to write JS to interact with a database
                (i.e., a JS \"layer\" on top of MongoDB). All of its
                operations could be written without Mongoose; however,
                Mongoose makes it easier to interact with MongoDB (in a
                similar way that jQuery makes it easier to interact with
                the DOM).

        -   **Installation**

            -   Install: npm install mongoose

            -   Require: var mongoose = require(\'mongoose\');

        -   **Configuration**

            -   **Connect**

                -   To connect Mongoose to your MongoDB, state the
                    following in your application:\
                    mongoose.connect(\'mongodb://localhost/cat_app\');

                    -   **NOTE**: If the specified database name (noted
                        as \"cat_app\" above) does not exist, then it
                        will automatically be created.

            -   **Schema**

                -   To add data to the database, it is first necessary
                    to define a schema (i.e., Mongoose\'s \"blueprint\"
                    of how a data object in a specific collection will
                    be organized). For example, if you are creating a
                    database of cats, the following schema could be
                    used:\
                    var catSchema = new mongoose.Schema({\
                    name: String,\
                    age: Number,\
                    temperament: String\
                    });

                    -   **NOTE**: You are not forbidden from adding new
                        properties or omitting the listed properties
                        when adding objects to the collection, but this
                        practice is not advised.

                    -   See here for a [**Complete List of Schema
                        Types**](http://mongoosejs.com/docs/schematypes.html).

                        -   The \"**Date**\" schema type can be
                            configured to automatically set its value to
                            be whatever the current time on the server
                            is upon execution, e.g.:\
                            var blogSchema = new mongoose.Schema({\
                            title: String,\
                            body: String,\
                            date: { type: Date, default: Date.now }\
                            });

                            -   **NOTE**: This option is not just
                                limited to date properties, but can be
                                applied in any situation in which you
                                want a **Default** value to be applied
                                where none is submitted by the user,
                                e.g.:\
                                image: { type: String, default:
                                \'defaultimage.jpg\' }

            -   **Model**

                -   Once a schema is defined, it can be complied into a
                    model (returned as an object with predefined
                    methods) that can be used to find, create, update,
                    and delete objects of the given type (once saved to
                    a variable), e.g.:\
                    var Cat = mongoose.model(\'Cat\', catSchema);

                    -   **NOTE**: The first argument must be the
                        **SINGULAR** name of the collection that will be
                        created for your model (Mongoose automatically
                        looks for the plural version of your model
                        name). The second argument is the schema you
                        want to use in creating the model.

                    -   **IMPORTANT**: The reason why \"Cat\" is
                        **CAPITALIZED** is because Mongo collection
                        names are case sensitive (\"Cats\" is different
                        from \"cats\"). Mongo convention calls for
                        having collection names pluralized and
                        lower-case, and model names singular and
                        upper-case. Thus, the collection would be called
                        \"cats\", whereas an individual model would be
                        called \"Cat\". Mongoose will automatically
                        lower-case and pluralize a model name so that it
                        can access the collection (i.e., \"Cat\" \>
                        \"cats\").

            -   **Module Exports**

                -   Rather than having your schema and model in the same
                    file as your app.js, you can break down each
                    schema-model pair into its own JS file and export
                    the output to app.js. This is useful for
                    modularizing those components that are likely to be
                    used in other projects.

                -   For example, if you have an online forum that would
                    have \"users\" and \"posts\" as schema and models,
                    you can create a separate JS file called \"user.js\"
                    for your user schema-model and a file called
                    \"post.js\" for your post schema-model. These files
                    should be placed in a directory called
                    \"**Models**\". You would configure your \"post.js\"
                    file as follows:\
                    var mongoose = require(\'mongoose\');\
                    var postSchema = new mongoose.Schema({\
                    title: String,\
                    content: String\
                    });\
                    **module.exports = mongoose.model(\'Post\',
                    postSchema);**

                    -   **NOTE**: Because the schema and model are
                        created by Mongoose, it is necessary to
                        \"require\" it for every file.

                -   The \"module.exports\" essentially operates as a
                    return function, which will export the returned
                    value of \"mongoose.model(\'Post\', postSchema)\" to
                    app.js once post.js is \"required\" in the app.js
                    file, e.g.:\
                    var Post = require(\'./models/post\');

                    -   **IMPORTANT**: When referencing directories in
                        Node, you must use **Dot-Slash** (./) to
                        reference the **CURRENT DIRECTORY**.

                -   **TIP**: Module exports are also useful for creating
                    a modular **Seeds File**, which can be used to
                    \"seed\" a database with placeholder data. Having
                    such a file to populate a database helps expedite
                    testing and debugging. The seeds file should be
                    \"required\" in the main app.js file and stored as a
                    variable that can be executed as a function, e.g.:\
                    var seedDB = require(\'./seeds\');\
                    seedDB();\
                    ... However, for this to work, it is necessary to
                    have the seeds file export a function, e.g.:\
                    function seedDB() {\
                    // insert code here\
                    }\
                    module.exports = seedDB;

        -   **Basic Methods**

            -   **Create & Save**

                -   Once a model has been compiled, you can create and
                    save **Documents**, which are instances (i.e., the
                    actual data object) of your model. You create the
                    document by first stating, e.g.:\
                    var newCat = new Cat({\
                    name: \'Stumpy\',\
                    age: 9,\
                    temperament: \'Timid\'\
                    });\
                    ...and then save the model by stating, e.g.:\
                    newCat.save(function(err, cat) {\
                    if (err) {\
                    console.log(err);\
                    } else {\
                    console.log(\'Saved:\');\
                    console.log(cat);\
                    }\
                    });

                    -   **NOTE**: The reason why you want to use the
                        callback function when saving is that you may
                        not always succeed if there is a problem with
                        the database or your connection to it. Because
                        this process may take time, it is worth having a
                        callback function let you know whether the
                        document saved or whether there was an error.
                        The first argument (any name you want, but
                        conventionally called \"err\") in the save
                        method\'s function contains any **Error**
                        message that was generated, and the second
                        argument (any name you want) contains the
                        **Result**, which is an object added to your
                        database (the data with an ObjectId).

                -   **BUT NOTE**: Rather than creating and saving in two
                    separate steps, it is possible to use the
                    **create()** method to perform both steps at once,
                    e.g.:\
                    Cat.create( {

> name: \'Koko\',
>
> age: 9,
>
> temperament: \'Silly\'
>
> }, function(err, cat) {
>
> if (err) {
>
> console.log(err);
>
> } else {
>
> console.log(\'Saved:\');\
> console.log(cat);
>
> }
>
> });

-   **Find**

    -   Once data has been added to the database, you can find data by
        using the following model method, e.g.:\
        Cat.find( {}, function(err, cats) {\
        if (err) {\
        console.log(err);\
        } else {\
        console.log(\'All Cats:\');\
        console.log(cats);\
        }\
        });

    -   The **find()** method can be used in tandem with Express to
        render a page that contains items in the database, e.g.:\
        app.get(\'/cats\', function(req, res) {\
        Cat.find( {}, function(err, allCats) {\
        if (err) {\
        console.log(err);\
        } else {\
        res.render(\'index.ejs\', { cats: allCats });\
        }\
        });\
        });

-   **Find By ID**

    -   While find() may return one or more documents that fit the
        specified criteria, **findById()** will only return the single
        document that matches the specified \_id. For example, if you
        want to render a page that shows the details about a specific
        cat in a collection, you can use a cat\'s \_id property as a URL
        parameter (e.g., www.mycats.com/cats/a1b2c3/, in which
        \"a1b2c3\" is the \_id), and access that parameter via
        Express\'s \"req.params.id\" statement and return the document
        object that matches said parameter through the following
        expression:\
        app.get(\'/cats/:id\', function(req, res) {\
        Cat.findById(req.params.id, function (err, foundCat) {\
        if (err) {\
        console.log(err);\
        } else {\
        res.render(\'show.ejs\', { cat: foundCat });\
        }\
        });\
        });

-   **Find By ID And Update**

    -   When making a **PUT** request, you can use the
        **findByIdAndUpdate()** method to concurrently find the ID of
        the document to be modified (as defined by the first argument),
        immediately modify the document with a new data set (as defined
        by the second argument), and execute a callback (as defined by
        the third argument). For example, if you used a form to edit the
        details about a specific cat in a collection, you can use
        \"req.params.id\" to return the matching document, and then run
        a command that will replace the data in that document with the
        new data object submitted in the body of the request (e.g.,
        \"req.body.cat\"):\
        app.put(\'/cats/:id\', function(req, res) {\
        Cat.findByIdAndUpdate(req.params.id, req.body.cat, function(err,
        updatedCat) {\
        if (err) {\
        console.log(err);\
        } else {\
        console.log(\'Updated:\');\
        console.log(updatedCat);\
        }\
        res.redirect(\'/cats/\' + req.params.id);\
        });\
        });

-   **Find By ID And Remove**

    -   The **findByIdAndRemove()** method operates in relatively the
        same manner as findByIdAndUpdate(), except there is no need to
        include a new body of information:\
        app.delete(\'/cats/:id\', function(req, res) {\
        Cat.findByIdAndRemove(req.params.id, function(err, deletedCat)
        {\
        if (err) {\
        console.log(err);\
        } else {\
        console.log(\'Deleted:\');\
        console.log(deletedCat);\
        }\
        res.redirect(\'/cats/\');\
        });\
        });

        -   **NOTE**: If you want to **REMOVE ALL DATA IN A
            COLLECTION**, use the **remove()** method with an empty set
            of curly brackets, e.g.:\
            Cat.remove({}, function(err) {\
            if (err) {\
            console.log(err);\
            } else {\
            console.log(\'Deleted all cats.\');\
            }\
            });

```{=html}
<!-- -->
```
-   **Data Associations**

    -   **Basics**

        -   A data association is a grouping of related elements. For
            example:

            -   You can have a group of users that each have a property
                to define the user\'s spouse. As each user can only have
                one spouse, and a spouse can be married to one user,
                this is a **One-To-One** association.

            -   You can have a group of businesses that each have a
                property to define the business\'s inventory. As each
                business can have multiple pieces of inventory (which
                can only belong to that one business), this is a
                **One-To-Many** association.

            -   You can have a group of students that each have a
                property to define the courses they are enrolled in, and
                a group of courses that each have a property of the
                students who are enrolled. As each student can be
                enrolled in multiple courses, and each course can enroll
                multiple students, this is a **Many-To-Many**
                association.

    -   **Embedding Data**

        -   Embedding data is one way of creating data associations. In
            the context of an online forum, you will have collections of
            users and collections of posts. With Mongoose, you can
            create schema for posts and users. To associate a post with
            a particular user, it becomes necessary to embed the post
            schema as an **Array** within the user schema, e.g.:\
            var postSchema = new mongoose.Schema({\
            title: String,\
            content: String\
            });\
            var Post = mongoose.model(\'Post\', postSchema);\
            var userSchema = new mongoose.Schema({\
            name: String,\
            email: String,\
            **posts: \[postSchema\]\
            **});\
            var User = mongoose.model(\'User\', userSchema);

            -   **NOTE**: For this process to work, the embedded schema
                must **PRECEDE** the schema in which it is embedded
                (i.e., the post schema must first be created before it
                can be embedded within the user schema).

        -   Once a user has been created and you want to associate a
            post with that user, you must (1) **Find** the user (most
            likely by using
            [**findOne()**](http://mongoosejs.com/docs/api.html#findone_findOne)
            if you are attempting to find by the user\'s name rather
            than ID), (2) **Push** the post to the user\'s \"posts\"
            property, and (3) **Save** the user, e.g.:\
            User.findOne({ name: \'John Smith\' }, function(err, user)
            {\
            if (err) {\
            console.log(err);\
            } else {\
            user.posts.push({\
            title: \'Post Title\',\
            content: \'Post Content\'\
            });\
            user.save(function(err, user) {\
            if (err) {\
            console.log(err);\
            } else {\
            console.log(\'Saved:\');\
            console.log(user);\
            }\
            });\
            }\
            });

            -   **NOTE**: The \"second\" user (in user.save) is
                different than the first (in User.findOne) due to scope.

    -   **Object References**

        -   An alternative way to create data associations is via object
            references. This method is similar to embedding data to the
            extent that data is stored in an array; however, rather than
            storing posts in their entirety, the array will only store
            **IDs** that are references to the post objects (instead of
            embedding the entire post), e.g.:\
            var userSchema = new mongoose.Schema({\
            name: String,\
            email: String,\
            **posts: \[\
            {\
            type: mongoose.Schema.Types.ObjectId,\
            ref: \'Post\'\
            }\
            \]\
            **});

            -   The syntax above essentially states that the user schema
                will contain a property called \"posts\", which is an
                array of object IDs (i.e.,
                mongoose.Schema.Types.ObjectId) belonging to a \'Post\'
                (\"ref\" refers to the model type that relates to the
                \"ObjectId\").

        -   Once the applicable schema and models have been established,
            you can concurrently (1) **Create** a post, (2) **Push** the
            post to a specified user, and (3) **Save** the modified user
            data, e.g.:\
            Post.create({\
            title: \'Post Title 2\',\
            content: \'Post Content 2\'\
            }, function(err, post) {\
            if (err) {\
            console.log(err);\
            } else {\
            User.findOne({ email: \'john\@smith.com\' }, function(err,
            user) {\
            if (err) {\
            console.log(err);\
            } else {\
            **user.posts.push(post.id);\
            ** user.save(function(err, user) {\
            if (err) {\
            console.log(err);\
            } else {\
            console.log(user);\
            }\
            });\
            }\
            });\
            }\
            });

        -   Once you have associated a collection of post IDs with a
            particular user, you can find the entire data content of all
            posts (rather than just IDs) associated with the user by
            stating the following:\
            User.findOne({ email: \'@\'
            }).**populate**(\'posts\').**exec**(function(err, user) {\
            if (err) {\
            console.log(err);\
            } else {\
            console.log(user);\
            }\
            });

            -   The syntax above essentially finds the specified user
                and then \"**Populates**\" the user\'s \"posts\"
                property with all of the data tied to all post IDs
                associated with the user. It is necessary to run
                \"exec()\" (**Execute**) at the end of the chain to
                actually start the query because no callback function is
                used in the \"findOne()\" method (as it would be
                pointless to do so until the \"posts\" property has been
                populated). The \"exec()\" method allows you to run the
                query after all the chaining and populating have
                finished.

```{=html}
<!-- -->
```

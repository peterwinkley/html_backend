-   [**RESTful (\"Representational State Transfer\")** **Routing**]{.ul}

    -   **Basics**

        -   RESTful routing can be described as an architectural style
            that provides a map between HTTP verbs (e.g., GET, POST,
            etc.) and CRUD (create, read, update, delete) actions. There
            are seven RESTful route conventions:

  **Name**   **URL**          **Verb**   **Description**                            **Mongoose Method**
  ---------- ---------------- ---------- ------------------------------------------ -------------------------
  INDEX      /dogs            GET        List all dogs.                             Dog.find()
  NEW        /dogs/new        GET        Show new dog form.                         N/A
  CREATE     /dogs            POST       Create a new dog (then redirect).          Dog.create()
  SHOW       /dogs/:id        GET        Show information about one dog.            Dog.findById()
  EDIT       /dogs/:id/edit   GET        Show edit (\"update\") form for one dog.   Dog.findById()
  UPDATE     /dogs/:id        PUT        Update one dog (then redirect).            Dog.findByIdAndUpdate()
  DESTROY    /dogs/:id        DELETE     Delete one dog (then redirect).            Dog.findByIdAndRemove()

-   RESTful conventions call for giving the CREATE route the **SAME
    NAME** as that given to the INDEX route that will be appended by the
    POST request. For example, if you have a route called \"/dogs\" that
    will display an array of dogs upon rendering \"index.ejs\" when a
    GET request is made, then you would similarly name the POST route
    \"/dogs\", even if the POST request is made from a different page
    (as it is ultimately appending the same array that will be displayed
    on \"/dogs\" pursuant to a GET request). The fact that both the GET
    and POST route have the same name will not create conflicts, as one
    is a GET and the other a POST. The same applies to SHOW, UPDATE, and
    DESTROY.

    -   **NOTE**: Each of the non-GET requests (i.e., POST, PUT, and
        DELETE) should always be followed up with a GET request
        redirect.

    -   **ALSO NOTE**: It is common for most websites to have their home
        page (\'/\') redirect to the route that renders the index,
        e.g.:\
        app.get(\'/\', function(req, res) {\
        res.redirect(\'/dogs\');\
        });\
        app.get(\'/dogs\', function(req, res) {\
        res.render(\'index.ejs\');\
        });

```{=html}
<!-- -->
```
-   **Nested Routes**

    -   When one collection of data will be associated with a document
        from another data collection, one should use nested routes. For
        example, in reference to the chart above, you may want to allow
        users to add comments on each dog\'s \"show\" route. This would
        require making a \"new\" route for the comment form, and then a
        \"create\" route to post comments. If the dog and comment routes
        were created independently of each other, you could setup the
        following routes:\
        NEW /dogs/new GET\
        CREATE /dogs POST\
        NEW /comments/new GET\
        CREATE /comments POST

    -   However, comments do not exist independently from dogs. When a
        comment is created, it is associated with a particular dog by
        its ID; therefore, it is essential to have the dog\'s ID in that
        particular route. Thus, the \"comments\" route should be nested
        under the \"dogs\" route by ID:\
        NEW /dogs/:id/comments/new GET\
        CREATE /dogs/:id/comments POST


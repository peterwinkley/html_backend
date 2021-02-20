-   [**Application Programming Interfaces
    (APIs)**](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)

    -   **Basics**

        -   APIs are interfaces for programs to interact with each
            other. It broadly refers to code that is made for other
            types of code to communicate with. Web APIs are more
            specialized for web applications (e.g., to have a program
            extract certain sets of information from Reddit), and they
            generally communicate via HTTP (via a URL).

    -   **XML and JSON**

        -   APIs don\'t respond with HTML (which contains information
            about the structure of a page). Rather, APIs respond with
            data (not structure) and therefore use simple data formats
            like XML and JSON.

        -   Extended Markup Language (**XML**) is similar to HTML in
            syntax but does not describe page presentation; rather it
            encodes key value pairs, e.g.:\
            \<person\>\
            \<age\>21\</age\>\
            \<name\>Travis\</name\>\
            \<city\>Los Angeles\</city\>\
            \</person\>

        -   JavaScript Object Notation (**JSON**) looks exactly like
            JavaScript objects, except everything is a **[STRING IN
            DOUBLE QUOTES]{.ul}** (no single quotes allowed), e.g.:\
            {\
            \"person\": {\
            \"age\": \"21\",\
            \"name\": \"Travis\",\
            \"city\": \"Los Angeles\"\
            }\
            }

            -   **IMPORTANT**: The fact that all data provided in a JSON
                **Body** is a string means that you cannot readily
                access the data in the same fashion as a traditional JS
                object. In order to turn the JSON body into an object,
                you must **Parse** it and save the data to a variable.
                JavaScript comes with a built-in parser for JSON, e.g.:\
                var data = JSON.parse(body);

        -   **NOTE**: By default, JSON is rendered as one large block of
            text in the Chrome browser. You can use the
            [**JSONView**](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en)
            extension to remedy this issue.

    -   **Making API Requests with Node (and Express)**

        -   The most common way to make an API request with Node is to
            install a package called
            [**Request**](https://www.npmjs.com/package/request). A
            sample request format is provided as follows:\
            var request = require(\'request\');\
            request(\'http://www.google.com\', function (error,
            response, body) {\
            // Print the **Error** if one occurred:\
            console.log(\'error:\', error);\
            // Print the response [**Status
            Code**](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
            if a response was received:\
            console.log(\'statusCode:\', response &&
            response.statusCode);\
            // Print the HTML **Body** for the Google homepage:\
            console.log(\'body:\', body);\
            });

        -   The above request() format can be placed into a GET request
            for the purpose of running the request once a user requests
            a certain page. Thus, for example, if a search form on the
            home directory takes the user to a \"/results\" subdirectory
            to view the results, then you would state:\
            app.get(\'/results\', function(req, res) {\
            // insert request() here\
            });\
            \...then any form that directs to \'results\' (via its
            \"action\" property) will submit all name-value pairs as
            properties that can be accessed within a single object
            called [**req.query**](http://expressjs.com/en/api.html) (in
            Express). These are the same name-value pairs that appear
            after the \"?\" in the URL. Thus, if a text input with the
            name of \"t\" (for title) and a numerical input with the
            name of \"y\" (for year) are used in a form, the submission
            of \"Frozen\" for the title and \"2013\" for the year would
            produce \"/results?t=frozen&y=2013\" in the URL, and this
            data can be accessed and assigned to a variable and used
            accordingly:\
            app.get(\'/results\', function(req, res) {\
            var title = req.query.t;\
            var year = req.query.y;\
            request(\'http://www.omdbapi.com/?apikey=thewdb&t=\' +
            title + \'&y=\' + year,\
            function (error, response, body) {\
            var data = JSON.parse(body);\
            res.render(\'results\', { data: data });\
            }\
            }\
            });

        -   **NOTE**: You can extract API data straight out of the
            command line by using:\
            curl \<insert API link here\>

-   **[Databases]{.ul}**

    -   **Basics**

        -   There are two broad categories of databases: **SQL**
            (sequel) and **NoSQL** (non-sequel).

            -   **SQL Basics**

                -   SQL databases are considered to be **Relational**
                    and organized in tabular form (a flat data
                    structure), e.g.:

  **USERS TABLE**                                  
  -------------------- --------------------- ----- ----------
  id                   name                  age   city
  1                    Tim                   57    NYC
  2                    Ira                   24    Missoula
  3                    Sue                   40    Boulder
  **COMMENTS TABLE**                               
  id                   text                        
  1                    \"First comment.\"          
  2                    \"Second comment.\"         
  3                    \"Third comment.\"          

-   If you want there to be a relationship between tables, you create a
    **Join Table**, e.g.:

  **USERS/COMMENTS JOIN TABLE**   
  ------------------------------- -----------
  userId                          commentId
  1                               3
  2                               1
  2                               2

-   **NoSQL Basics**

    -   By contrast, NoSQL databases are **Non-Relational** and
        organized in BSON (Binary JSON) form (a nested data structure),
        e.g.:\
        {\
        name: \"Tim\",\
        age: 24,\
        city: \"Missoula\",\
        comments: \[\
        { text: \"First comment.\" },\
        { text: \"Second comment.\" }\
        \]\
        }

```{=html}
<!-- -->
```
-   **NOTE:** NoSQL is newer than SQL, but not necessarily better in all
    cases. Other popular databases that are SQL include MySQL and
    PostgreSQL.

```{=html}
<!-- -->
```

-   [**MongoDB**](https://www.mongodb.com/)

    -   **About**

        -   MongoDB is the most popular NoSQL database. MongoDB is
            currently the most popular database for Node as part of the
            \"**MEAN**\" stack, which stands for (1) MongoDB, (2)
            Express, (3) Angular, and (4) Node.

    -   **~~Installation and Use (FOR CLOUD9)~~** **\[OUTDATED\]**

        -   ~~Preliminarily, installation requires running the following
            command:\
            sudo apt-get install -y mongodb-org~~

        -   ~~Subsequently, run the following commands (refer to the
            above link for more information about the parameters used):\
            mkdir data\
            echo \'mongod \--bind_ip=\$IP \--dbpath=data \--nojournal
            \--rest \"\$@\"\' \> mongod\
            chmod a+x mongod~~

    -   **Installation and Use (FOR CLOUD9) \[v.3.6.2\]**

        -   Preliminarily, installation requires running the following
            commands in order:

1)  sudo apt-key adv \--keyserver hkp://keyserver.ubuntu.com:80 \--recv
    2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5

2)  echo \"deb \[ arch=amd64 \] https://repo.mongodb.org/apt/ubuntu
    trusty/mongodb-org/3.6 multiverse\" \| sudo tee
    /etc/apt/sources.list.d/mongodb-org-3.6.list

3)  sudo apt-get update

4)  sudo apt-get install -y mongodb-org

    -   Subsequently, run the following commands in order:

```{=html}
<!-- -->
```
1)  mkdir data

    -   **NOTE**: This directory is where Mongo stores its data.

2)  echo \"mongod \--dbpath=data \--nojournal\" \> mongod

3)  chmod a+x mongod

    -   Start Mongo by running the \"mongod\" script (the Mongo Daemon)
        on your project root:\
        ./mongod

        -   **IMPORTANT**: Once mongod is running, **THE TERMINAL
            RUNNING MONGOD MUST REMAIN OPEN FOR THE DATABASE TO REMAIN
            UP**. Thus, you must create a new terminal for performing
            all other tasks while the mongod terminal remains untouched
            in its own terminal.

        -   **HOWEVER**: Once you are done working on your project,
            **SHUT DOWN MONGOD**, or else it can crash if/when Cloud9
            times out.

    ```{=html}
    <!-- -->
    ```
    -   **Basic Mongo Commands**

        -   **Mongo Shell**: mongo

            -   Opens the shell, which is used for testing and
                debugging.

        -   **Help**: help

            -   Displays a list of Mongo\'s basic features.

        -   **Show** databases: show dbs

            -   Displays a list of all databases (the two default
                databases are \"admin\" and \"local\").

        -   **Use** or make a database: use

            -   If a database does not exist, you can use the use
                command to create a database; otherwise, the command
                will use the database, e.g.:\
                use demo

                -   **NOTE**: All databases are empty by default and
                    will not appear on the show dbs list until there is
                    data within them.

        -   **Add** data (by creating a new \"**Collection**\" or adding
            to an existing one): insert

            -   All data are stored in groups called collections. If,
                for example, you want to create a collection of data
                about dogs, you could create a collection called
                \"dogs\" and add data to it concurrently by stating:\
                db.dogs.insert( {name: \'Rusty\', breed: \'Mutt\'} )

                -   **NOTE**: Once the collection is created, additional
                    data can be added to the collection by using the
                    same syntax above:\
                    db.dogs.insert( {name: \'Lucy\', breed: \'Mutt\'} )\
                    db.dogs.insert( {name: \'Lulu\', breed: \'Poodle\'}
                    )

                -   **ALSO NOTE**: When an object is added to a
                    collection, Mongo automatically assigns each object
                    its own unique **ObjectId** in hexadecimal.

        -   **Find** (or \"reading\") data in a collection: find

            -   Displays either (1) a list of all objects in a given
                collection, or (2) a specific object within a given
                collection. As to the first operation, you can state the
                following:\
                db.dogs.find()\
                \...which will return ALL objects in the collection.
                However, if you want to find a specific object within
                the collection, you need only pass through one property
                as an argument:\
                db.dogs.find( {name: \'Rusty\'} )

        -   **Modify** or add new properties to an existing object in a
            collection: update

            -   Modifies an object\'s property by passing through two
                arguments in which you (1) first specify a unique
                property that identifies a particular object to be
                modified (e.g., a \"name\" or \"\_id\"), and (2) then
                modify the value of a specific property within that
                object. For example, you can change Lulu\'s breed from
                \"Poodle\" to \"Labradoodle\", and also add a new
                Boolean property called \"isCute\" by saying:\
                db.dogs.update( {name: \'Lulu\'}, {\$set: {breed:
                \'Labradoodle\', isCute: true} } )

                -   **IMPORTANT**: If you do not use \"\$set\" and
                    simply say:\
                    db.dogs.update( {name: \'Lulu\'}, {breed:
                    \'Labradoodle\', isCute: true} )\
                    \...then you will **OVERWRITE ALL PROPERTIES** in
                    the object with whatever properties are contained in
                    the second argument (which, in this case, would
                    delete Lulu\'s name property).

        -   **Delete** **DATA** from a collection: remove

            -   Removes all matching objects according to the following
                syntax:\
                db.dogs.remove( {breed: \'Mutt\'} )\
                ...which, in this case, would result in the deletion of
                both Rusty and Lucy, because they are both \"Mutt\"
                breeds.

                -   **NOTE**: The default behavior is to delete all
                    objects that match; however, the number can be
                    limited by using the **Limit** method:**\
                    **db.dogs.remove( {breed: \'Mutt\'} ).limit(1)

        -   **Delete AN ENTIRE COLLECTION**: drop

            -   Removes an entire collection:\
                db.dogs.drop()

    -   **Security**

        -   For information regarding **Security Best Practices**, refer
            to the following links:

            -   [Security Best Practices for Express in
                Production](https://expressjs.com/en/advanced/best-practice-security.html)

            -   [MongoDB Security Best
                Practices](https://www.mongodb.com/blog/post/how-to-avoid-a-malicious-attack-that-ransoms-your-data)

    ```{=html}
    <!-- -->
    ```

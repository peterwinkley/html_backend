    -   [**mLab**](https://mlab.com/)

        -   **About**

            -   mLab (formerly known as \"MongoLab\") is a fully-managed
                cloud database service that hosts MongoDB databases.
                mLab will provide you with a persistent URI to your
                hosted MongoDB database that will replace the
                \"localhost\" connection link in your Heroku-hosted
                application.

        -   **Usage**

            -   After creating an account on mLab, access your dashboard
                for \"**MongoDB Deployments**\" and select \"**Create
                New**\".

            -   After creating a database, you will receive a **URI** to
                be used in connecting your application to the database,
                e.g.: mongodb://\<dbuser\>:\<dbpassword\>\
                \@ds123456.mlab.com:12345/your-database-name

            -   Upon receiving the URI, it is necessary to add a
                database user. Select the \"Users\" tab and select
                \"**Add Database User**\".

            -   After creating a database user, **Replace** \<dbuser\>
                and \<dbpassword\> in the above URI in your application
                with the username and password.

            -   Finally, add and commit the changes to your application,
                and push to Heroku.

    -   **Environment Variables**

        -   In the section entitled \"Listening\" under \"Express.js\"
            above, it is noted that the following code must be included
            in the application:\
            app.listen(process.env.PORT, process.env.IP);

        -   The above-referenced \"PORT\" and \"IP\" are environment
            variables, which are values that can vary depending on the
            environment in which the application is being run.

        -   When deploying an application with a database, it is
            beneficial to use an environment variable in reference to
            your database **URI**. For example, you may have your
            application hosted on Cloud9 for testing, and also hosted on
            Heroku for deployment. However, if each copy has its data
            hosted on the same mLab database, then modifications made in
            testing will also carry over to the deployed application.

        -   To ensure that your testing and deployment applications
            connect to separate databases, you must convert the value in
            the following line:\
            mongoose.connect(\'mongodb://localhost/your_application\');\
            ...to an environment variable, e.g.:\
            mongoose.connect(\'process.env.DATABASEURI\');

            -   **NOTE**: As a matter of convention, environment
                variables are typed in **ALL CAPS**.

        -   Once this change has been made, you can then set the value
            of DATABASEURI in **Cloud9** by using the **Export**
            command:\
            export DATABASEURI=mongodb://localhost/your_application\
            ...and you can set the value in **Heroku** by running the
            following in the Cloud9 console:\
            heroku config:set DATABASEURI=
            mongodb://\<dbuser\>:\<dbpassword\>\
            \@ds123456.mlab.com:12345/your-database-name

            -   **NOTE**: Alternatively, you can set environmental
                variables in the Heroku dashboard by going to
                \"**Settings**\" and selecting \"**Config Vars**\".

            -   **SEE** [**Heroku Node.js
                Support**](https://devcenter.heroku.com/articles/nodejs-support)
                for more information on deploying a Node.js application
                on Heroku.

        -   **TIP**: Environment variables are also useful for
            concealing sensitive information.

        -   **NOTE**: If you want to have a **Backup URI**, you can use
            the following procedure, e.g.:\
            var uri = process.env.DATABASEURI **\|\|
            \'mongodb://your/backup/uri\'**;\
            mongoose.connect(uri);

            -   In the event that the value of DATABASEURI is undefined,
                your application will still connect to the backup URI
                that has been explicitly provided.

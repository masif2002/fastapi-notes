# Simple Navigations in pgAdmin
pgAdmin is the GUI we use to interact with Postgres databases.

## Creating a server
* Right click **Server**, then **Create > Server**
* Give a name for the server
* Switch to the **Connection** tab. In the Host name/address field, we give the IP address that he postgres instance runs on 
    * Since, for now it is running on our local machine, we give **localhost**.
    * If you are running it on a cloud, the service provider will give an address or a domain which you specify in this field.
* Enter the password for the default user.
* Click **Save** and your server is created.

## Creating a Database
* Go to **Servers > name_of_your_server > Databases**.
* Under databases, we would already have one default postgres instance, that was created for us in the installation process.
* Right click **Databases**, then **Create > Database**.
* Give the name for the database.
* Click **Save** and your database is created.

## Creating a Table
* Go to **Servers > name_of_your_server > Databases > name_of_your_database > Schemas > public > Tables**.
* Right click **Tables**, then **Create > Table**.
* Give the name for the table
* Go to the columns section and create columns.
* Click **Save** and your table is created.

## Viewing the entries in tables
* Right click the name of your table, then **View/Edit Tables > All Rows**.
* After adding a new entry, you need to click **Save Data Changes** available on the top in the form of a database symbol or press **F6**.
    * Unsaved data is represented in bold characters.
* If you still need to update any properties in the table, or add a column or whatever, you can go to **Properties** when you right click the name of your table.

## Writing own queries
* Right click the name of your database and then **Query Tool**.
* After typing out the query, hitting the run button on top will execute it.
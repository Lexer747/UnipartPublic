# Website Documentation

**Foreword**: If you just want a basic overview of how to adjust the website
to certain graphs see: [Add a new graph](#adding-a-graph) or for more detail
[How graphs are displayed.](#queryinfo-tag---mandatory)

---

For a more intermediate view read this page. Finally for the more advance view
read this current page and this [Technical Website Documentation](https://github.com/Lexer747/Unipart/blob/Alex/Website/Client/README.md). 
For editing more of the site than just the config I suggest you read all of the
code aswell as these two pages.

## Adjusting the Website

Alot of the functionality of the website is set in stone, such as layout
and looks. These things can only be changed by reading the more complex
information listed above. Although lots of things can be changed without 
advance knowledge, Features which can be changed easily are:

*   [How graphs are displayed.](#queryinfo-tag---mandatory)
    
    * [Adding a new graph](#adding-a-graph)
    
    * [Adding a trace to an existing graph](#adding-a-trace)

    * [Ordering the drop down](#ordering-the-drop-down)
    
*   [What is shown in the "Raw Data" table.](#defaultid-tag---mandatory)

*   [Adjusting the Database](#database-tag---mandatory)

These changes are easy to make as they can all be done by changing the 
`config.xml`.

# Using the config.xml file to make changes

Note about config.xml location: The config.xml is required by the website to be 
in this folder:

```
Unipart\Website\Client\WebContent\resources\
```

**It cannot be anywhere else!**

## Overall structure

This diagram should explain the rough idea of the xml file, more detail
about each tag is found after the diagram.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<resources>
    <database>
        <linux>/home/Database.db</linux>
        <!-- or <windows> tag but at-least one -->
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

The file above is most bare bones your config can be and still work. **Hence
this shows all the non-optional tags!** Therefore for the website to work your
config **must have at-least these tags** and with the relevant information.

So your file **must** have a path to the database file, it **must** also have
at-least one query with the `<defaultID>` tag pointing to that only query.

---

Optional tags are for most part related to queries so that will be covered
in the query tag section. The only important tag optional tag is
operating system tag. You **must** have at least one OS specified and
a path to a database inside that tag, you can have **multiple** OS's 
specified and the website will detect which one to use **automatically**.

### Database tag - mandatory
The database file used by the website can be stored **anywhere** you like as long
as the path of the file is **specified** inside the repsective tag.

The database file used in development was `SQLite version 3.19.3` aslong
as you file is atleast `SQLite version 3.x` it should probably be fine.
However if you are having trouble this could be an easy fix.

The actual layout of the database **internally** can be **anything** you would 
like aslong as the `<queryInfo>` tag is properly configured.

If you are running a **windows** machine use the `<windows>` tag. And use the 
`<linux>` tag for any **linux or mac** machine.

---

### Linux tag - optional

Linux is very simple to change the location of the database. Simply copy
the **explicit, fully qualified path name** to your `sqlite3` database file.

e.g.
``` xml
<database>
    <linux>/home/pi/Documents/Unipart/Website/Database/Database.db</linux>
</database>
```

Note: **be careful with extra whitespace and new lines**

---

### Windows tag - optional

Windows is slightly more complicated as the search for the file starts at
```
C:\Users\%YOURUSERNAME%\Desktop\
```
Where `%YOURUSERNAME%` is the name of windows user.

So this path is at the start of any path you then provide in the `<windows>`
tag. This means if your database is at `C:\Database.db` then the path in
the tag will be: `\..\..\..\Database.db`.

A more real world example is as such:

* Full path name for database = 
`C:\Users\%YOURUSERNAME%\Documents\Work\Unipart\Website\Database\Database.db`

* Therefore the path needed in the tag =
`\..\Documents\Work\Unipart\Website\Database\Database.db`

* Full tag in xml:
``` xml
<database>
    <windows>\..\Documents\Work\Unipart\Website\Database\Database.db</windows>
</database>
```

Note: **be careful with extra whitespace and new lines**

## DefaultID tag - mandatory

This tag contains an **ID** of one of the **queries** specified in the
`<queryInfo>` tag and the query **specified** is the one that is used for the 
"Raw Data" table.

So if you wish to **customize the table** you should do so by either using a
different ID or **customizing the actual query**.

---

The only cavet not mentioned in the query tag section is that the first
column heading always shown in the table, i.e.

``` SQL
SELECT x, y, z FROM table
```

In the case above the `x` column will always be on show.

## QueryInfo tag - mandatory
 
Inside this tag should be **every graph** you wish to display on the website, 
and a query which is used to display the "Raw Data" table. In this tag is
where the **majority of the customization** can be done. 

---

<h5>Every single *syntactially correct* graph query will appear as an option
in the drop down and have its *corresponding graph plotted*.</h5>

---

## Query tag - optional

The `<query>` tag has a **unique attribute** for every query which is a
**non-negative integer**, its ID. So every query in the xml file can be indentified
and then sorted in the dropdown based off its ID.

*  Every opening tag should be as follows: `<query ID="x">`

---

The tag also holds all the information needed to draw a graph or the table.
Although it is optional, at-least one is needed for the table. There are
actually two types of query tag, a **graph query** and a **normal query**.

A normal query **will not** appear on the graph dropdown automatically, but
graph queries **will** show up on the dropdown automatically. Because of this
a graph query requires extra information inside of its tag.

element|normal query| graph query
---|---|---
`ID="x"`|**mandatory**|**mandatory**
`<SQL>`|**mandatory**|**mandatory**
`<title>`|optional|**mandatory**
`<xColumn>`|optional|**mandatory**
`<yColumn>`|optional|**mandatory**
`<xaxis>`|optional|**mandatory**
`<yaxis>`|optional|**mandatory**

---

## SQL tag - mandatory

This tag is **required** inside every query tag and should hold the literal SQL
string that will be excuted. So if you wish to **display every peice of data**
in the table then your query should **encapsulate** that.

i.e.
``` xml
<query ID="0">
    <SQL>SELECT Time_stamp, Cooling_Water_Temperature FROM Main;</SQL>
</query>
```

for the table every column that is selected **will be shown**, and the first
column is always shown.

## title tag - optional

This tag is required mostly for debugging purposes, but it is mandatory
for a graph query. The information inside the tag will be the **title 
of the graph**.

## xColumn tag - optional

This tag is only used if the query is a graph. **Only the first
`<xColumn>` tag can be used, adding more invalidates the query**.
This tag should tell the graph which column of database should be plotted
along the x-axis.

i.e.

I would like to plot **Time_name** against **Temperature_name** then the
query **could** be as such:

``` SQL
SELECT Time_name, Temperature_name FROM table;
```

And to specify which column should be plotted on the **x-axis** we use this tag:

``` xml
<xColumn>Time_name</xColumn>
```

Note:**This is not the name used to label the x-axis!** the string inside
the tag should be the literal column name in the select statement.

## yColumn tag - optional

This tag is only used if the query is a graph. Unlike the `<xColumn>` 
tag you can **have as many `<yColumn>` as you like**. Each one will be
plotted on the graph **together**. This is useful if you want to compare
data from two or more columns.

---

The `<yColumn>` tag also has a `name` attribute which is only used
when more than `<yColumn>` tag is used. It is used to label the trace on
the graph, if it is not provided then the trace defaults to the actual
value of the tag.

i.e.

I would like to plot **Time_name** againt **Temperature_name1** and
**Temperature_name2** then the query could be as such:

``` SQL
SELECT Time_name, Temperature_name1, Temperature_name2 FROM table;
```

And to specify which columns are to go on the y-axis we use the tag as
such:

``` xml
<yColumn name="Helpful name 1">Temperature_name1</yColumn>
<yColumn name="Helpful name 2">Temperature_name2</yColumn>
```
Note:**This is not the name used to label the y-axis!** the string inside
the tag should be the literal column name in the select statement.

## x-axis tag - optional

This tag is only used if the query is a graph. This tag should hold the
**name of x-axis** that is actually labelled on the graph.

## y-axis tag - optional

This tag is only used if the query is a graph. This tag should hold the
**name of y-axis** that is actually labelled on the graph.

# Graph annotated

This image and annotations demonstrate how a graph can be added by 
specifing it inside the config. Assuming that there is actually
data in the database.

![](https://github.com/Lexer747/Unipart/blob/Alex/Website/images/Graph.png)

Full xml that generated this graph:

``` xml
<query ID="14">
    <SQL>SELECT Time_stamp, Pressure_Intensifier_X_Acceleration, Pressure_Intensifier_Y_Acceleration, Pressure_Intensifier_Z_Acceleration, Pressure_Intensifier_Resultant_Acceleration FROM Main;</SQL>
    <xColumn>Time_stamp</xColumn>
    <yColumn name="X">Pressure_Intensifier_X_Acceleration</yColumn>
    <yColumn name="Y">Pressure_Intensifier_Y_Acceleration</yColumn>
    <yColumn name="Z">Pressure_Intensifier_Z_Acceleration</yColumn>
    <yColumn name="Resultant">Pressure_Intensifier_Resultant_Acceleration</yColumn>
    <xaxis>Time</xaxis>
    <yaxis>Acceleration (ms -2)</yaxis>
    <title>Pressure Intensifier Overall</title>
</query>
```

As you can see the actual data points come from the `<SQL>` tag.

* The blue highlighted title comes from:

``` xml
<title>Pressure Intensifier Overall</title>
```

* The red highlighted legend comes from:

``` xml
<yColumn name="X">Pressure_Intensifier_X_Acceleration</yColumn>
<yColumn name="Y">Pressure_Intensifier_Y_Acceleration</yColumn>
<yColumn name="Z">Pressure_Intensifier_Z_Acceleration</yColumn>
<yColumn name="Resultant">Pressure_Intensifier_Resultant_Acceleration</yColumn>
```

* The green and brown axis titles comes from:
``` xml
<xaxis>Time</xaxis>
<yaxis>Acceleration (ms -2)</yaxis>
```

## Adding a graph

To add a new graph you need to add `<query>` tag inside the `<queryInfo>`
tag. And to make it a graph you need to also add the related graph
tags.

Here is the base config we will use to demonstrate how to add a graph.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

1. Add the `<query>` tag to the `<queryInfo>` tag. **Do not forget the ID!**

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
        
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

2. Add the actual SQL to fetch the data.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData FROM DatabaseTable;</SQL>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

3. Add the tags to specify which data is on the x-axis and which data is on
the y-axis. **These tags must match the actual column names from the query!**

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

4. Add the tags to label the x and y axis, these are the names to actually
show to the user.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data (other units)</y-axis>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

5. Finally add the title of the graph.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data (other units)</y-axis>
            <title> Data plotted </title>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

## Ordering the Drop down

The order in which the graphs are shown is in numerical, so the query with
ID 0 is first, then ID 1, then ID 2, etc...

* How do the dividers appear on the dropdown?

The website will put a divider between two graphs if the titles are
significantly different. So if you want graphs to be grouped together
make thier titles more similar, if you want a divider make the titles
less similar.

For detail on how the website determines "similarity" see the function
`isSimilar` in `GraphHelper.java`.

## Adding a trace

Adding a trace is easy, we will use the base xml from the previous section to
demonstrate. All we need to do is add another `<yColumn>` tag to the existing
graph.

Starting config:
``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data (other units)</y-axis>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

1. Change the SQL to get the new data.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData, moreData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data (other units)</y-axis>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

2. Add the new column to the tags

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData, moreData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn>someOtherData</yColumn>
            <yColumn>moreData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data (other units)</y-axis>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

3. Name each trace so that the user can tell them apart. And change the
y-axis name.

``` xml
<resources>
    <database>
        <linux>/home/Database.db</linux>
    </database>
    <queryInfo>
        <query ID="0">
            <SQL>SELECT * FROM DatabaseTable;</SQL>
        </query>
        
        
        <query ID="1">
            <SQL>SELECT someData, someOtherData, moreData FROM DatabaseTable;</SQL>
            <xColumn>someData</xColumn>
            <yColumn name="Some Other Data">someOtherData</yColumn>
            <yColumn name="More Data">moreData</yColumn>
            <xaxis>Some Data (units)</xaxis>
            <yaxis>Some Other Data + More Data (other units)</y-axis>
        </query>
        
        
    </queryInfo>
    <defaultID>0</defaultID>
</resources>
```

Now both columns of information are displayed on the same graph.
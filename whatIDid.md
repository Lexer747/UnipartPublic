# Timeline:

* Problems that I was asked to solve.

    * Someway to display the data
    
    * Someway to store the data
    
    * Assuming the data comes from another source
    
## Database Based Design - A way to store data

By using a database it made it easy to seperate the "view" of the data from
the data collection and management. It is common practice to use the 
**'model view controller' (MVC)** structure for many programs. So
it was a natural choice to have the database as the most flexible yet powerful
*controller* in which we store our data.

The **controller** defines how the view is allowed to interact with the model.
It is a layer of seperation and abstraction so that the view does not
have to worry about managing induvidual sensors. It only has to take
data from a database.

This was the first step in the creation of the website as it was of critical
importance that we have a way for our two seperate programs to interact.

## Web Based Display - A way to display the data

Any program could have displayed the data in a graph from the database. But
only webapps have the advantage of working on every device with no
new software needing to be installed. Additionally having to add a
monitor to the machine to show the graphs would have been expensive and
impractical. So a webapp made the most sense for **view** part
of the **MVC** pattern.

---

Therefore by assuming the data is coming from another source. And that the
source will be handling the **model** part of **MVC**. This means 
that we have all the parts needed to make our program.

### Diagram

``` 

         Model                 Controller              View
  _________|_________         _____|______         _____|_____
 |                   |       |            |       |           |
 |  External Source  | <---> |  Database  | <---> |  Web App  |
 |___________________|       |____________|       |___________|
  
```

This diagram was our initial thoery behind the overall program. Akshay could focus
on collecting data from the machines and passing it to the database. While
I could focus on getting data from the database and displaying it.

---

# Initial prototyping

Turns out it is very easy to extract data from the database and display it. So
the first prototype was setup relatively quickly:

![](https://github.com/Lexer747/Unipart/blob/Alex/Website/images/v1.png)

All it did was prove that it could read this data from the database. But 
that was an important step in and of itself

## Fleshing out the prototype

The next step was to attempt to plot the data on graph. This was
done with the help of a **3rd party libary** which makes drawing the graphs
much easier.

![](https://github.com/Lexer747/Unipart/blob/Alex/Website/images/v2.png) 

The graph is very bare, but once again this was just proof that it could
be done.

# Makeing the web app look better

Now that the ground work was in-place it was a matter of simply
makeing the web app look alot nicer. This was done using **bootstrap
and javascript**.

![](https://github.com/Lexer747/Unipart/blob/Alex/Website/images/v3_1.png) 

![](https://github.com/Lexer747/Unipart/blob/Alex/Website/images/v3_2.png) 

# Adding Customizabilty via a Configuration File

We realized later into the development of the project that if we were to
change the **internal strucutre of the database** the website would also
need to change to represent that. So instead of that a **configuration
file** is used to tell the website which graphs to draw and how to
communictae with the database.
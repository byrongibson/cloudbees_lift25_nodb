<a href="https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_nodb/master/clickstart.json">
<img src="https://s3.amazonaws.com/cloudbees-downloads/clickstart/clickstart-now.png"/></a>

# Scala 2.9 and Lift 2.5 ClickStart.

![Lift Logo][10]

The [Lift Framework][1] is a popular web framework for the [Scala][2] Programming language. 
It is known for being [very secure out of the box][3], scalable, and mature.  It makes 
ajax and comet [easy][4], and is "full stack" (ie comes with pretty much everything you 
need, including ORM). Success stories include large public sites such as [Foursquare][5] 
and sophisticated web applications like [Novell Vibe][6] and [Innovation Games][7].

![Scala Logo][11]

This ClickStart bootstraps you with a working barebones lift web application, a database 
([H2][8]), a source repository (populated, ready to go), and a Jenkins build service 
running continuous deployment from the source repository (push a change, your project 
will be built and deployed). 

This Clickstart uses the latest version of [Lift 2.5][16], currently Lift 2.5-RC2, 
and will be updated as new versions are released.  It is based on CloudBees' 
[default Lift 2.4 ClickStart][9] but does *not* include a MySQL instance, only the 
default [H2 DB][8] integrated into Lift.  Please see the [MySQL][19] and [PostgreSQL][20] 
Lift 2.5 ClickStarts if you prefer one of those databases, or [CloudBees' docs on how
to make your own ClickStart][17] if you prefer something else.

You can use this as a starting point for your own lift application (remember the source 
repository will be private to your account). 

[Click here][13] to launch this right now.

Feel free to fork and make this your own - pull requests welcome !


## Manual steps for deploying on CloudBees:

Create application:

    bees app:create MYAPP_ID

Create database:

    bees db:create -u DB_USER -p DB_PASSWORD DBNAME

Bind database as datasource:

    bees app:bind -db DBNAME -a MYAPP_ID -as LiftDB

Create a new maven project in Jenkins, changing the following:

* Add this git repository (or yours with this code) on Jenkins
* Also check "Deploy to CloudBees" with those parameters:

        Applications: First Match
        Application Id: MYAPP_ID
        Filename Pattern: target/*.war

## To build this locally:

In the lift_template directory, open a command line, and invoke maven by typing "mvn 
package" to build the war file, then deploy it on cloudbees typing:
	
    bees app:deploy -a MYAPP_ID target/*.war

## To run this locally:

Modify the src/main/scala/bootstrap/liftweb/Boot.scala file by commenting the following line:

    DefaultConnectionIdentifier.jndiName = "jdbc/LiftDB"

And uncommenting the following:

    val vendor = new StandardDBVendor(Props.get("db.driver") openOr "org.h2.Driver", 
    Props.get("db.url") openOr "jdbc:h2:lift_proto.db;AUTO_SERVER=TRUE",
    Props.get("db.user"), Props.get("db.password"))
    LiftRules.unloadHooks.append(vendor.closeAllConnections_! _)
    DB.defineConnectionManager(DefaultConnectionIdentifier, vendor)

Then finally run with jetty type "sbt update ~jetty-run" in the project directory, and then browse to localhost:8080



[1]:    http://www.liftweb.net/
[2]:    http://scala-lang.org
[3]:    http://seventhings.liftweb.net/security
[4]:    http://seventhings.liftweb.net/comet
[5]:    http://www.foursquare.com
[6]:    http://vibe.novell.com/
[7]:    http://innovationgames.com/
[8]:    http://www.h2database.com
[9]:    http://github.com/CloudBees-community/lift_template 
[10]:   http://upload.wikimedia.org/wikipedia/commons/b/b7/Lift-logo.jpg "Lift Logo"
[11]:   http://upload.wikimedia.org/wikipedia/en/8/85/Scala_logo.png "Scala Logo"
[12]:   https://s3.amazonaws.com/cloudbees-downloads/clickstart/clickstart-now.png "Launch ClickStart"
[13]:   https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_nodb/master/clickstart.json
[14]:   https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_mysql/master/clickstart.json
[15]:   https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_pgsql/master/clickstart.json
[16]:   https://github.com/lift/lift_25_sbt
[17]:   https://developer.cloudbees.com/bin/view/RUN/How+to+make+your+own+Clickstart
[18]:   https://github.com/byrongibson/cloudbees_lift25_nodb
[19]:   https://github.com/byrongibson/cloudbees_lift25_mysql
[20]:   https://github.com/byrongibson/cloudbees_lift25_pgsql

[]:     https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_nodb/master/clickstart.json
[]:     https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_mysql/master/clickstart.json
[]:     https://grandcentral.cloudbees.com/#CB_clickstart=https://raw.github.com/byrongibson/cloudbees_lift25_pgsql/master/clickstart.json

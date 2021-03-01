# NOAH: Interactive Spreadsheet Exploration with Dynamic Hierarchical Overviews

_NOAH_ is a dynamic hierarchical overview that can be embedded alongside spreadsheets to explore the data at various granularities by zooming in and out of the spreadsheet. The overview is essentially a histogram with bins and depicts the overall data distribution, enabling users to quickly assimilate the data. Using NOAH users can issue formulae over data subsets without cumbersome scrolling or range selection, enabling users to gain a high or low-level perspective of the spreadsheet. NOAH enables users to customize the overview and also automatically manages user's exploration history to help them retrace previous steps in the analytics session. _NOAH_ is integrated within [DataSpread][dataspread-github], a _spreadsheet-database hybrid system_ with a spreadsheet frontend and a database backend. Following is a video demo of NOAH:

[![nav-demo](https://user-images.githubusercontent.com/8811607/109452683-2d65bf00-7a05-11eb-81a4-14caf5a480e0.png)](https://www.youtube.com/watch?v=iZsboe3x680 "Navigation Demo")


### Key Features

* _Navigation_: Users can navigate the spreadsheet by interacting with the bins of the overview via clicking and semantic zooming operations. Details [here](navigation.md).
* _Aggregation_: Users can issue spreadsheet formulae on the overview and view the results within an _aggregate column_. Details [here](aggregation.md).
* _Customization_: Users can perform various custimizations such as merging or splitting bins of the overview. Details [here](customization.md).
* _Context Management_: Users can view their navigation history in a context bar and navigate the hierarchical overview using a breadcrumb. Details [here](context.md).


# Setup Instructions

NOAH can be deployed locally through Docker (recommended) or through Apache Tomcat. 

## Method 1: Build and Deploy Using Docker

### Required Software

* [Docker][docker] >= 1.13.0

### Deploying NOAH locally.

1. Clone the NOAH repository and go the directory in your terminal. Alternatively, you can download the source as a zip or tar.gz. 

2. Install Docker. [Docker][docker] makes it easy to separate applications from underlying infrastructure so setting up and running applications is quick and easy.

3. Start Docker and start the application. It should be accessible at [http://localhost:8080/][install_loc]. Stop the application with `CTRL+C`.
	```
	docker-compose up
	```

## Method 2: Build and Deploy Using Maven+Tomcat

To host NOAH locally on Tomcat, you can either use one of the pre-built WAR files, available at `/build-web/DataSpread.war`, or build the WAR file yourself from the source.

### Required Software

* [Java Platform (JDK)][java] >= 8
* [PostgreSQL][postgres] >= 10.5
* [PostgreSQL JDBC driver][jdbc] = 42.1.4
* [Apache Tomcat][tomcat] >= 8.5.4
* [Apache Maven][maven] >= 3.5.0
* [NodeJS][node] >= 10.9

### Building Instructions (To generate a WAR file)

1. Clone the NOAH repository. Alternatively, you can download the source as a zip or tar.gz. 

2. Use maven to build the `war` file using the following command.  After the build completes, the WAR is available at `webapp/target/DataSpread.war`. 

	```
	mvn clean install
	```

### Deploying NOAH locally 

1. Install PostgreSQL database. [Postgres.app][Postgres.app] is a quick way to get PostgreSQL working on Mac. For other operating systems check out the guides [here][postgre_install].  

2. Create a database and an user who has access to the database.  Note the database name, username and password. Typically when you have PostgreSQL installed locally the password is blank.  

3. Install Apache Tomcat. You can use the guide [here][tomcat_install]. Make a note of the directory where tomcat is installed. This is known as `TOMCAT_HOME` in all documentation. 

4. Update the Tomcat configuration. You need to update the following file, which is present in `conf` folder under `TOMCAT_HOME` folder.  

    1. `context.xml` by adding the following text at the end of the file before the closing XML tag.   

	```
	<Resource name="jdbc/ibd" auth="Container"
	          type="javax.sql.DataSource" driverClassName="org.postgresql.Driver"
	          url="jdbc:postgresql://127.0.0.1:5432/<database_name>"
	          username="<username>" password="<password>"
                  maxTotal="20" maxIdle="10" maxWaitMillis="-1" defaultAutoCommit="false" accessToUnderlyingConnectionAllowed="true"/>
	```

	Replace `<database_name>`, `<username>` and `<password>` with your PostgreSQL's database name, user name and password respectively.

5. Copy `postgresql-42.1.4.jar` (Download from [here][jdbc]) to `lib` folder under `TOMCAT_HOME`.  It is crucial to have the exact version of this file. 
 
6. Deploy the WAR file within Tomcat as the root application. This can be done via Tomcat's web interface by undeploying any application located at `/` and deploying the WAR file with the context path `/`. To do this manually, delete the `webapps/ROOT` folder under `TOMCAT_HOME` while the application is not running, copy the WAR file to the `webapps` folder, and rename it to `ROOT.war`. 

## Launching NOAH 
After step 3 of Method 1 or step 6 of method 2, you are ready to run the program. Visit the url which is typically [http://localhost:8080/][install_loc] for a local install. Upload a file and start navigation using the following steps:

### Menu Selection  
From the menubar select Nav -> Explore. 

![exploreS1](https://user-images.githubusercontent.com/8811607/109452997-d3b1c480-7a05-11eb-8f25-15739e589ea7.png)

### Attribute Selection
Select the attribute by which you want to explore the data.

![exploreS2](https://user-images.githubusercontent.com/8811607/109452999-d8767880-7a05-11eb-8351-7f5c9abdfb5c.png)

### Overview Display
The overview is displayed within a _Navigation Panel_ on the left (see `a` in the following Fgiure). Users can perform various operarion on the overview such as [navigation](navigation.md), [aggregation](aggregation.md), [customization](customization.md), and [revisting navigation history](context.md). 

<img width="953" alt="navigation" src="https://user-images.githubusercontent.com/8811607/109453457-fabcc600-7a06-11eb-9fc7-f85d9cd52037.png">

# Technical Details
For more details on _NOAH_ read out technical paper at [VLDB 2021][paper]. Cite our work as follows: 
```
@article{rahman2021noah,
  title={NOAH: Interactive Spreadsheet Exploration with Dynamic Hierarchical Overviews},
  author={Rahman, Sajjadur and Bendre, Mangesh and Liu, Yuyang and Zhu, Shichu and Su, Zhaoyuan and Karahalios, Karrie and Parameswaran, Aditya},
  journal={Proceedings of the VLDB Endowment},
  volume={14},
  number={6},
  pages={970--983},
  year={2021},
  publisher={VLDB Endowment}
}
```


License
----
MIT

[install_loc]: http://localhost:8080/
[tomcat_install]: https://www.ntu.edu.sg/home/ehchua/programming/howto/Tomcat_HowTo.html
[postgre_install]: https://wiki.postgresql.org/wiki/Detailed_installation_guides
[Postgres.app]: http://postgresapp.com
[jdbc]: https://repo1.maven.org/maven2/org/postgresql/postgresql/42.1.4/postgresql-42.1.4.jar
[ant]: https://ant.apache.org/bindownload.cgi
[tomcat]: http://tomcat.apache.org/download-80.cgi
[java]: http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html
[postgres]:https://www.postgresql.org/download/
[siteinfo]: http://kite.cs.illinois.edu:8080
[zksite]: https://www.zkoss.org/product/zkspreadsheet
[postgressite]: https://www.postgresql.org/
[dataspread-github]: http://dataspread.github.io
[dataspread-site]: http://data-people.cs.illinois.edu/dataspread.pdf
[maven]: https://maven.apache.org/install.html
[node]: https://nodejs.org/en/download/current/
[docker]: https://www.docker.com/products/docker-desktop
[wiki]: https://github.com/dataspread/dataspread-web/wiki
[issues]: https://github.com/dataspread/dataspread-web/issues
[developer_setup]: https://github.com/dataspread/dataspread-web/wiki/Setting-up-Developer-Environment
[paper]: http://www.vldb.org/pvldb/vol14/p970-rahman.pdf

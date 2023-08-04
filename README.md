# Migrating from Mobiscroll Calendar to Bryntum Calendar: Back end

Install the dependencies by running the following command:

```bash
npm install
```

Run the server locally using the following command:

```bash
npm run dev
```

In the `utils/dbConnect.js` file, the Express server uses the MySQL2 library to connect to the MySQL database. A connection pool is created using the MySQL2 `createPool` method. The 
The connection pool is exported into the `server.js` file where it is used to run database queries for CRUD operations using the `query` method.

Now create a `.env` file in the root folder and add the following lines for connecting to the MySQL database that we’ll create:

```
HOST=localhost
PORT=1338
USER=root
PASSWORD=
DATABASE=mobiscroll
FRONTEND_URL=http://localhost:5173
```

Don’t forget to add the root password for your MySQL server.

## Setup a MySQL database locally

We’ll install MySQL Server and MySQL Workbench. MySQL Workbench is a MySQL GUI that we’ll use to create a database with tables for the Calendar data and to run queries. Download MySQL Server and MySQL Workbench from the MySQL community downloads page. If you’re using Windows, you can use the MySQL Installer to download the MySQL products. Use the default configurations when configuring MySQL Server and Workbench. Make sure that you configure the MySQL Server to start at system startup for your convenience.

Open the MySQL Workbench desktop application. Open the local instance of the MySQL Server that you configured.

We’ll write our MySQL queries in the query tab and execute the queries by pressing the yellow lightning bolt button.

### Create a MySQL database for the Mobiscroll Calendar data: Adding tables and adding example data

Let’s run some MySQL queries in MySQL Workbench to create, use, and populate a database for our Mobiscroll Calendar data. Execute the following query to create a database called mobiscroll:


```sql
CREATE DATABASE mobiscroll
```

Run the following query so that we set our newly created database for use:

```sql
USE mobiscroll;
```

Let’s create the event table that we’ll need for our Mobiscroll Calendar data:

```sql
create TABLE `bryntum_calendar_events` (
  `id`                      int           NOT NULL AUTO_INCREMENT,
  `name`                    varchar(255)  NOT NULL,
  `readOnly`                boolean       DEFAULT FALSE,
  `resourceId`              int           DEFAULT NULL,
  `timeZone`                varchar(255)  DEFAULT NULL,
  `draggable`               boolean       DEFAULT TRUE,
  `resizable`               varchar(255)  DEFAULT null,
  `children`                varchar(255)  DEFAULT null,
  `allDay`                  boolean       DEFAULT FALSE,
  `duration`                float(11, 2)  unsigned DEFAULT NULL,
  `durationUnit`            varchar(255)  DEFAULT 'day',
  `startDate`               datetime      DEFAULT NULL, 
  `endDate`                 datetime      DEFAULT NULL,
  `exceptionDates`          json          DEFAULT null,
  `recurrenceRule`          varchar(255)  DEFAULT null,
  `cls`                     varchar(255)  DEFAULT null,
  `eventColor`              varchar(255)  DEFAULT null,
  `eventStyle`              varchar(255)  DEFAULT null,
  `iconCls`                 varchar(255)  DEFAULT null,
  `style`                   varchar(255)  DEFAULT null,
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=1;
```

The fields with a type of `json` are fields that may be objects. MySQL does not have an object field type so we’ll use the `json` type instead. The data for these fields will need to be stringified before it's inserted into the database and parsed when it's retrieved from the database.

Now add some example events data to the events table:

```sql
INSERT INTO `events` (id, start, end, title) VALUES (1, '2023-07-25T12:00', '2023-07-25T14:00', 'Lunch at the office'), (2, '2023-07-25T14:10', '2023-07-25T16:00', 'General orientation'), (3, '2023-07-25T16:10', '2023-07-25T18:00', 'Stakeholder meeting'), (4, '2023-07-25T09:00', '2023-07-25T11:30', 'Management meeting');
```

You’ll be able to view the example events data by running the following query:

```sql
SELECT * FROM events;
```
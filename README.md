HebEngEventCal
==============

PHP calendar of events with Hebrew and Gregorian dates
------------------------------------------------------

This project is a php class that functions as a plugin, displaying a combined Hebrew/Gregorian date calendar with links to events pulled from a database or RSS feed. Currently only MySql databases are supported, with RSS support planned in the near future.

Two unique features of this plugin are that 1.) included in the class is an option to display the table layout and caption in Hebrew or English, configurable for best match to the website it's being displayed on, and 2.) the calendar doesn't display by month, but starts from today and shows a set number of days in the future, ideal for an 'upcoming events' calendar.

Implementation
--------------

1. Install the class wherever you deem convenient in your file structure, by cloning the repo and copying it to your server.  The repo contains only two files: hebEngCal.php, which contains the class, and the calendar.css file, which has default style definitions for the calendar.

2. Include the 'hebEngCal.php' on the web page you want to use the calendar on, like this: `include "hebEngCal.php";

3. Instantiate the class and configure the database login info and additional details for the class to function properly, as explained below:

The class contains the following properties:

		string  tableName			// name of table in database where data is stored for upcoming events 
		string  dateField			// field in said table that contains the event date
		string  dateFormat		// format of date in dateField, according to [PHP DateTime formatting](http://php.net/manual/en/datetime.createfromformat.php). If the date format includes a timestamp, the timestamp needs be truncated in order for the events to show up in the calendar
		string  linkURL			// URL of the page the links lead to, with room for the event identifier: 'http://mysite.com/events.php?id='. Not necessary if linkIDField contains full URL for event page
		int 	  days				// Number of days the calendar should display, starting from the present date. Default value is 60
		boolean eventText			// Determines whether or not to display event title text in the calendar dates (TRUE for large, full page calendar, FALSE for compact calendar in the sidebar). Default value is FALSE
		string  eventTextField	// Field in the table where event titles are stored. Not necessary if eventText = FALSE
		boolean hebTitles			// Determines what language calendar caption will display in, and what direction the days will follow. Default value is TRUE (Hebrew, rtl)
		string  linkIdField		// Field in the table that contains a unique identifier, or unique URL, for each event
		Array	  dbArray			// Array containing database login details, in the following order: host, database name, user name, password
		
All the properties can be set individually, or in the constructor. The constructor looks like this:

	public function __construct($tableName="",$dateField="",$linkIdField="",$dateFormat="",$linkURL="",$dbArray=Array(),$days=60, $hebTitles=TRUE,$eventText=FALSE,$eventTextField="",$method="RSS")

Methods

The class contains only two methods, `setDbParams(Array $dbArray)`, used to pass the database login parameters if they weren't set in with the constructor.
The second method is the printCal() method. It takes no parameters, and should be called only after all the properties have been set.

In addition to the PHP, the CSS file includes selectors for all the various components of the calendar, as well as default style definitions.  The default is for a small, text-less Hebrew (rtl) calendar.

Code Example
------------

	<?php
			$myCal = new HeEnEventCal();
			$myCal->tableName="events";
			$myCal->hebTitles=FALSE;
			$myCal->eventText=TRUE;
			$myCal->eventTextField="Title";
			$myCal->dateField="Date";
			$myCal->linkURL="http://mysite.com/eventcalendar.php?id=";
			$myCal->linkIdField="eventID";
			$myCal->dateFormat='d-M-Y';
			$dbArray = array("localhost","myDB","myuser","mypass");
			$myCal->setDbParams($dbArray);
			$myCal->printCal();
	?>
	
Live Demo
---------
A live demo is available [here](http://jegesh.github.io/hebEngCal/index.php)

Known Issues
------------

In order for the events to be visible when the calendar is small and doesn't display event text, it is necessary to add client-side code to color the background of the calendar dates a different color if they contain an event. To that end, this line of jQuery is sufficient:

	$("td").has("a.cal_link").css("background-color","#E090F0"); // of course, you can change the color to whatever you like
	
This can also be used for a larger calendar, making the event dates more immediately visible.


There is currently no MySql/SQL error handling implemented, but it is planned for the near future.

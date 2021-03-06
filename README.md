# uOttawa Schedule Generator

A schedule generator for the University of Ottawa written in Java, using OCSF. It is a tool to help register for courses, as the University-supplied program, "Rabaska", does a terribly poor job at helping students find conflict-free schedules.

## Introduction

This project was created by Ted Morin & Daniel Murdoch as assignment 5 & 7 for the course SEG2105A at the University of Ottawa. The program is now maintained by David Schlachter.
We created this program based on one simple problem line:
University of Ottawa students have no easy way to generate a conflict-free, optimized schedule.

Now that that class is done, we are continuing development on the project, as it is a useful utility when working with uOttawa's Rabaska course-registration system.

## [Download](https://github.com/morinted/schedule-generator/releases/download/v1.1.1/uOttawa-Schedule-Generator-v1.1.1.jar)

If you just want to get started generating schedules right away, then [download the program for Windows, Mac, and Linux](https://github.com/morinted/schedule-generator/releases/download/v1.1.1/uOttawa-Schedule-Generator-v1.1.1.jar). Note that you need Java to run the .jar file. Check out the user's guide below if you get confused while using the program.

## Features

### Current Features

Some features currently present:

- A console and graphical user interface.
- Select mandatory courses, but can edit sections.
- Select x of y optional courses, ideal for electives.
- Hosting a server at schlachter.ca to let individuals connect with the GUI.
- Export .ics files for easy calendar integration.
- Sorting by various attributes, including start time, days off, and length of day.
- Can ignore discussion groups and tutorials when sorting (eg. A day with only a discussion group would be considered a day off)

### Future Plans

These are problems that exist or features that are to be implemented. Consider this a bug list:

- GUI: When connecting to server, the message box can get caught behind the main window.

## User's Guide

### Getting Started

1. To start, it is recommended to download the program's front-end. This can be found in the releases section under the name [uOttawa-Schedule-Generator-v1.1.1.jar](https://github.com/morinted/schedule-generator/releases/download/v1.1.1/uOttawa-Schedule-Generator-v1.1.1.jar).

2. Run the .jar.

	Simply double-click the .jar file to run. If you don't have Java installed, you can get it from [Sun's website](http://java.com/en/download/index.jsp) or by running [Ninite](http://ninite.com) (my preferred choice, as there is no chance of the silly "Ask Toolbar"). Note that on macOS you may need to open the program by right-clicking it and selecting Open, depending on your Gatekeeper settings.

3. Wait for the server to connect, then select a semester from the drop down.

4. Start typing a course code in the search text box.

5. Select a course from the search results. You can click "Add Selected Course", double-click the course, or, if there is only one search result, hit enter.

6. If you'd like to select a number of courses from a list of electives, check the "Optional?" option and add your electives. For example, you can add 10 electives, then say that you want to choose 2 of them, and your schedule will reflect this. The number of electives you'd like to choose is at the bottom of the left pane, changed with the "-" and "+" buttons.

	Mandatory courses or electives you definitely want should *not* be using the optional option. The function is for when you are looking at which electives are the most convenient for your schedule.

7. After selecting your courses, you may choose to omit certain sections or activities (like DGDs) based on their location, professor, or if they're full. To do this, double-click your course or click the course and select "Edit". You can use the check-boxes in the pop-up window to change your selection.

8. Select a sort order: Earliest start, shortest days, least days per week, whatever. You choose what matters most to you. By default, all other sort orders have a secondary sort of least days per week. Meaning that if two schedules have the same average starting time, the one with the least days will be favored.

9. Click generate! You will receive the schedules on the calendar, which you can browse through.

10. You can view the text-formatted schedule (for easy entry into Rabaska) or export an .ICS file (for import into Google Calendar)

### Google Calendar Import

After exporting an iCalendar file, you will want to import it into a program. Microsoft Outlook makes it simple to just open the file, but Google Calendar has a slightly more involved process.

Note: You'll want to create a new calendar, just in case you want to delete the events created in this process. If you add it to your regular calendar and there is an error, there is no quick "Undo" function.

1. Create a new Calendar on the left side-bar. Next to "My calendars", click the drop-down and select "Create New Calendar". Assign it a snazzy color and name it something suave like "uOttawa F2014".

2. Click "Create Calendar"

3. In the drop-down next to "Other calendars", select "Import calendar"

4. On this pop-up, hit "Choose File" and select the .ICS file generated by the schedule-generator.

5. Select the calendar you just created in the drop-box, and hit "Import"

6. Enjoy your newly-generated calendar! Notice that the section, course code, course name, professor, and location are included in the events.

	We don't account for holidays, so make sure you don't come to school during Study Week!

### Hosting a Server

To host a server, download the entire project, and use ServerConsole.jar (in the releases section). Note that the ServerConsole depends on the ../course-download/db_*.csv files. So for this reason, you cannot run the ServerConsole.jar as a standalone.

To launch, you use the command: `java -jar ServerConsole.jar [port]`

Where [port] is the port to host on. If omitted, the default of 5555 will apply.

The server will create a server.stat file when schedules are generated that keeps track of how many users connect, how many schedules are generated, as well as the types of generations.

### Connecting to Different Server or Port

ClientGUI and ClientConsole can take several launch commands. In this example, the UI will be named `schedule-generator.jar`. It applies for both the GUI and the console.

In the working directory of the jar file, run `java -jar schedule-generator.jar [host] [port]`

Both optional, host lets you specify a server to connect to. The default is schlachter.ca.

Port lets you specify the port to connect on, the default, if omitted, is 5555.

### Updating Course Database

The timetables are stored in a .CSV, and follow a specific format. Given a list of course codes, the script should download the timetables off the [uOttawa website](http://www.timetable.uottawa.ca). The output .CSV is appropriately formatted to be read by the schedule generator server.

Originally, the script was a macro-enabled Excel spreadsheet using VBA to download the pages, but it was slow and the error messages were vague and difficult to pinpoint. Sometimes the script would freeze, too, with no indication of what was happening.

Now we use a set of Python scripts. One gets all the course names using Selenium webdriver, and the other updates all the timetables based on those courses.

In order to run these scripts, make sure to have Selenium and Chromium installed.

To update courses, use: `py .\getCourses.py`

After the script finishes (watch the console output), run the timetable script.

To update timetables, use: `py .\getTimetables.py`

Depending on your connection, these scripts take ~10-20 minutes combined to run for the full ~4000 courses. In contrast, the Excel script took roughly 3 hours.

**Remember to use Python 2.7+ and not 3.**

(Tip: On a headless server, you can use `xvfb` to run Chromium without a graphical session, e.g. `xvfb-run ./getCourses.py`)

## Screenshots

### The main UI, with a sample schedule already generated:

![The main UI, with a sample schedule already generated.](https://github.com/morinted/schedule-generator/raw/master/Documentation/Screenshots/mainui.png)

### The schedule above after being exported to an .ICS and imported into Google Calendar:

![The schedule above after being exported to an .ICS and imported into Google Calendar.](https://github.com/morinted/schedule-generator/raw/master/Documentation/Screenshots/googlecalendar.png)

## Dependencies

The schedule generator was made using the [OCSF](http://www.site.uottawa.ca/school/research/lloseng/supportMaterial/ocsf/ocsf.html) client-server interface. The iCalendar support is provided by [Biweekly](http://sourceforge.net/projects/biweekly/)

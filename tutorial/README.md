# Transition tutorial

This tutorial will guide you through the first steps of using [Transition](http://transition.city) and help you discover what can be achieved with it.

It supposes you have access to an instance of Transition, with access to a routing server (osrm) that includes the region that you want to analyze. See the [project's development page](http://github.com/chairemobilite/transition) for instructions to install Transition.

## Lesson 1: Import existing data from GTFS

[GTFS](https://gtfs.org/) is a open standard format used to distribution transit information. Most agencies in the world use this format, that can be used by various tools to show transit data to users.

Many transit feeds can be easily found in the [Open Mobility data](https://transitfeeds.com/) website, but the most up to date versions are recorded in the [Mobility Database catalogs](https://database.mobilitydata.org/). The transit agencies website may also contain this information. For example, the Montreal Transportation Society, whose data has been used to write this tutorial, can be found on [their website](https://www.stm.info/en/about/developers).

To import the GTFS, click on the `Import from a GTFS feed` menu item on the left, towards the bottom of the list. It will open the import panel.

![Import GTFS Menu](images/gtfsImportMenu.png)

Select the GTFS zip file that contain the data to import

Click on the upload file button ![Upload GTFS file button](images/gtfsUploadFileButton.png)

It will then display the agencies and services available on the GTFS. Selecting an agency will display the available lines. Select the agencies and lines to import. You can click the `Select all` button to import all lines for the selected agency.

![Import GTFS: Select agencies and lines](images/gtfsSelectAgenciesAndLines.png)

Select the services to import. Service names are not always obvious, some agencies have a lot of services. They can either all be select, or filtered to include only the services for weekdays at a given date.

![Import GTFS: Filter services](images/gtfsFilterServices.png)

If there are still many services, you may check the "Merge services for the same periods?" checkbox at the end of the service list. It will merge all the services that are for the same days and dates as one.

For the periods groups, selecting `Default` will cause the schedules to be split into 6 periods: early morning, peak AM, day, peak PM, evening and night. Transition is used for transit planning, so schedules are not meant to be fine-tuned by the minute, but rather calculated as a batch for periods. This allows to generate schedules every 15 minutes in the early morning, every 5 minutes during peak AM, etc. If periods are not important, for example, to have lines run all day every 10 minutes, then choose the `Complete day` period.

![Import GTFS: Merge services and import data](images/gtfsImportData.png)

Finally, click the `Import data`. This will upload the transit data into Transition. It may take a few minutes to complete depending on the agency's size. If the import completes without error, you will be redirected to the `Agencies and lines` panel. 

If there are errors, the import tab will remain open and display the errors and warnings. They may not all result in bad routing, but may prevent correct display of involved lines. Those lines may need to be investigated with care.

*If you know that the GTFS is valid, don't hesitate to open a new issue on [the Transition github](https://github.com/chairemobilite/transition), with a description, the error message and a link the GTFS file used.*

But at this point, there should be data available in Transition!

![Import GTFS complete](images/gtfsImportComplete.png)

## Lesson 2: Prepare imported data for calculations

Once the data is imported, it is available in the `Agencies and lines` panel. The `Stop nodes` will also display all the stop nodes locations imported with the data, as shown in the screenshot below.

![Lesson 2: agencies, lines and stop nodes](images/lesson2_agenciesLinesStopNodes.png)

But it is not yet ready to be used in calculations. The following steps will prepare the newly imported data so that we can simulate and analyze it.

### Create scenario

A scenario in Transition represents a transit network, a state of the transit offer, for a certain date. It can group many services, from many agencies. It can also include only or exclude some data (lines, modes, etc). All calculations (routing, accessibility maps, etc) are made against a scenario.

To create a new scenario, click on the `Scenarios` menu item on the left. It will open the `Scenarios` panel.

![Lesson 2: scenario menu](images/lesson2_scenariosMenu.png)

Then click on the `New scenario` button. Give a name for this scenario.

Then select the services to include in this scenario. If the services were filtered at import, you may select them all, otherwise, only the services for the day to test should be selected. All trips for the selected services will be used for routing, so mixing weekday and weekend services will artificially increase the offer!

![Lesson 2: scenario edit](images/lesson2_scenarioEdit.png)

The map shows the lines that are part of this scenario. Also, they are listed at the bottom of the panel, after the actions buttons.

![Lesson 2: display scenario data](images/lesson2_scenarioDisplayData.png)

Click on the save button ![save button](images/buttonSave.png) to save the scenario.

### Refresh calculation data

Some data needs to be pre-calculated, to avoid lengthy calculations. When the transit data has been edited and is ready for calculation, do the following steps:

1. Refresh the transferrable nodes: The walking distance from each nodes to all other nodes is calculated (up to a max of 20 minutes). This is used to determine possible transfer times and distances during a trip.

2. Save data to cache for the routing engine: Transition uses [trRouting](https://github.com/chairemobilite/trRouting) as the routing engine. `trRouting` reads its data from a cache that needs to be manually updated when there are changes to the transit data in Transition.

3. Start the routing engine: By default, when the instance starts, the routing engine is not running. It needs to be manually started. 

Usually, the icons will be yellow when there are un-processed data and will turn white once it the action has been executed. The following screenshot shows the buttons for each step. *These manual steps are error prone and we are working to make them automatic.*

![Lesson 2: refresh calculation data](images/lesson2_refreshCalculationData.png)

The data is now ready for calculation!

## Lesson 3: Test the imported data by calcuting routes

## Lesson 4: Visualize accessibility maps

## Lesson 5: Edit scenarios, to remove modes or lines

## Lesson 6: Edit existing lines

### Edit existing line, to add a few stops

### Update schedules for the edited line

### Validate results with routing

## Lesson 7: Create new services/agencies/lines and validate results

### Create new service

### Create new nodes

### Create new agency

### Create new line

### Create paths for the line

### Create schedule for the new line

### Create a new scenario to include this new line

### Validate results with routing

## Lesson 8: Export transit data as GTFS

## Lesson 9: Batch calculations
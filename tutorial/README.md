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

When changes are done on a scenario, before doing lengthy calculations on it, it is important to test that it works as expected and give logical results. This can be done by testing some trip calculations on the transit network.

Navigate to the `Routing` panel, by clicking on the `Routing` in the left menu.

![Lesson 3: routing menu](images/lesson3_routingMenu.png)

You can fine-tune the parameters, to change the departure/arrival time, maximum travel time, minimum waiting time, maximum access, egress and transfer time.

Then select the scenario to test and whether you want alternatives or not.

To define the origin and destination, you may simply click on the map. The first click will define the origin, all further clicks will change the destination.

To change the origin again, right-click on the map. A popup menu will open and allow to select the origin or destination for the current point localtion.

Finally, click on the ![calculation button](images/buttonSave.png) calculation button and you should see the result of the route display on the map.

![Lesson 3: display routing results](images/lesson3_displayRoutingResults.png)

When selecting results with alternatives, you can navigate through the alternatives to see the detailed results for each.

![Lesson 3: routing results with alternatives](images/lesson3_resultsWithAlternatives.png)

### Troubleshooting routing

The routing engine returns no result? 

If there is no access, either at origin or destination (no stop nodes near those points), or if there is no service at origin or destination, the error message will be explicit enough. You can change the origin or destination for locations where there is service available.

![Lesson 3: routing results error no access](images/lesson3_resultsErrorNoAccess.png)
![Lesson 3: routing results error no service](images/lesson3_resultsErrorNoService.png)

If the message says it returned no result, it will hint to look at some parameters values. You can try changing the travel time, access/egress, etc.

![Lesson 3: routing results error no routing](images/lesson3_resultsErrorNoRouting.png)

If you are certain that there should be a route with the given parameters, go back to [lesson 2](#lesson-2-prepare-imported-data-for-calculations) and validate that the scenario contains the expected lines, then refresh calculation data to make sure it is up to date.

## Lesson 4: Visualize accessibility maps

Another type of calculation that can be done with the scenario is the accessibility maps. Accessility maps are isochrones displaying the region accessible in transit within a certain amount of time.

Navigate to the `Accessibility map` panel, by clicking on the `Accessibility map` in the left menu.

![Lesson 4: accessibility map menu](images/lesson4_accessibilityMapMenu.png)

Many of the parameters to set are similar to those of the `Routing`. Specifying a departure time means the selected location is an origin, while an arrival time means the location is a destination.

The parameters specific to the accessibility map are the number of polygons and the delta and delta interval. The number of polygons is the number of isochrones that will be drawn. The outermost isochrone is for the maximum travel time specified. And each other isochrone is for a corresponding fraction of the total time. For example, 4 polygons with a 60 minutes total travel time will draw isochrones for 15, 30, 45 and 60 minutes.

As for the delta, it allows to flatten the variabilities at different times by calculating an average of the accessibility a bit before and a bit after the requested time. For example, with the default values of departure time at 8:00 and a delta of 10 minutes and interval of 5 minutes, it will calculate the accessibility at 7:50, 7:55, 8:00, 8:05 and 8:10. A node `n` may be accessible in 15 minutes at 7:50, 10 minutes at 7:55, 20 minutes at 8:00, 15 minutes at 8:05 and 10 minutes at 8:10. The final time for node `n` will be the average of all the times obtained (`(15 + 10 + 20 + 15 + 10) / 5 = 14` minutes), while it would have been 20 minutes if only the 8:00 calculation had been taken into account.

Select the scenario, as for the routing calculation, then click on the ![calculation button](images/buttonSave.png) calculation button and you should see the result of the route display on the map. Results may take a while to calculate. You can decrease the maximum total travel time or the number of polygons to speed things up to quickly see a first result.

![Lesson 4: display accessibility map results](images/lesson4_displayAccessibilityMapsResults.png)

### Troubleshooting accessibility maps

If there is a problem with the scenario or the selected locations, there won't be any error message, but the only isochrone displayed will be a circle of walking distance from the point, as shown in the screenshot below. That means that no transit trip could be done from this location.

![Lesson 4: walking isochrone only](images/lesson4_walkingIsochroneOnly.png)

It may mean that the point is outside the network zone or the time outside the service hours. Or there may be problem with the network. Make sure that `Routing` works as expected from/to this location. See [the `Routing` troubleshooting section](#troubleshooting-routing) for information to troubleshoot transit errors in calculations.

So far, we have learned how to create a scenario from a service or set of services and how to test this scenario by making single calculations on it to make sure it corresponds to what is expected. We will now learn to modify the scenarios or network and validate the results.

## Lesson 5: Edit scenarios, to remove modes or lines

We will learn to fine-tune a scenario, to remove certain modes or lines from the calculations.

Go back to the `Scenario` panel (![Scenario panel icon](images/buttonScenarioPanel.png)).

We will duplicate the scenario that we created earlier, by clicking on the duplicate button ![Scenario duplicate button](images/buttonDuplicate.png) next to the scenario. This will create a scenario of the same name, with the `(copy)` suffix.

![Lesson 5: duplicate scenario](images/lesson5_scenarioDuplicateButton.png)

Click on this scenario to edit it. Rename it to something more explicit, for example `Test STM without metro`.

To exclude the metro, select the `Metro/Subway/Aerial metro` in the `Excluded modes` fields. You may also remove a few lines in the `Excluded lines` field.

![Lesson 5: edit scenario remove modes and lines](images/lesson5_removeModesAndLinesFromScenario.png)

Then click on the ![save button](images/buttonSave.png) save button to save this scenario.

Finally, for the new scenario to be considered, you must save the data to cache for the routing engine (step 2 of the [refresh calculation data section](#refresh-calculation-data)) by clicking on the ![save data to routing cache](images/buttonSaveDataToCache.png). This will automatically restart the routing engine.

### Validate results with routing

The scenario is now ready for calculation. You can now go back to the `Routing` and `Accessibility maps` panels, select the new scenario and make sure the metro modes and excluded bus lines are not part of the transit trips anymore.

![Lesson 5: Routing with the scenario without metro](images/lesson5_routingWithoutMetro.png)

Notice that without the metro, for a very similar trip than the one calculated in [lesson 3](#lesson-3-test-the-imported-data-by-calcuting-routes), there are now 44 alternatives instead of just 3. But the first alternatives in this scenario takes *66 minutes* instead of just around 30 minutes in the original scenario. Alternatives are considered valid if they are within a factor (1.75) of the first alternative returned. So this 66 minutes trip would not have been considered as an alternative to a 30 minutes trip, it is way too long!

![Lesson 5: Compare travel time of first alternatives](images/lesson5_compareTravelTimeOfAlternatives.png)

As we see, the number of alternatives alone should not be considered as a metric to validate the quality of service of a network, as it depends on the best alternative, which, in the case of a metro line available, is much better than any other alternative!

## Lesson 6: Edit existing lines

In this lesson, we will learn to modify existing lines, update their schedules and validate the results again. We will extend a metro line and modify a bus line along this metro line. 

### Edit existing line, to add a few stops

To see all current lines, go to the `Agencies and Lines` panel, from the left menu and expand the lines by clicking the `Lines` text under the agency name.

![Lesson 6: Agencies and lines panel](images/lesson6_agenciesAndLines.png)

Select the metro line that will be edited (here, the Montreal STM's blue line). It will open the line edit panel, with the inbound and outbound paths for this line.

![Lesson 6: Line edit panel](images/lesson6_lineEditPanel.png)

Click on one of the paths to edit. They will both be edited, so any will do for now. The entire path is selected and a bottom panel opens to show the stops of the path. At the top left of the bottom panel is a help button ![path edit help button](images/buttonPathEditHelp.png) that shows information on how to edit a path. Click on it to display the help functions.

![path edit bottom panel](images/lesson6_pathEditBottomPanel.png)

We will extend the lines towards the northeast. Stop nodes will need to be added either at the beginning or at the end of the line, depending on its direction. 

To add nodes at the end, simply click on the nodes, in order. To add the nodes at the beginning, shift-click on the node to add.

Notice that since metro lines are underground, they do not follow the road network and the line will between 2 nodes will be a straight line.

![Lesson 6: straight lines between metro stations](images/lesson6_straightLinesBetweenNodes.png)

Now click on the save button ![save button](images/buttonSave.png) to save this path.

Do the same operation for the reverse path, adding nodes at the end or beginning of the path and saving it. The paths list in the line should now show more stops on both paths.

![Lesson 6: modified path list](images/lesson6_modifiedPathList.png)

### Update schedules for the edited line

Now that paths have been changed, the schedules should also be updated. To do so, in the `Line edit` panel, click on the `Timetables` button to open the timetable panel on the left.

![Lesson 6: open timetables](images/lesson6_openTimeTables.png)

Select the current service to open the current schedule. This will display all periods for the day. The current schedule comes from the imported GTFS file, so each trip may be at different times. In transit planning, fine-tuning each trip is usually not required, so we will re-create the schedule for each period with a certain interval.

For example, our metro service starts at 5:30 and will be every 15 minutes until 6AM, then it will increase to 7 minutes from 6AM to 9AM, the every 10 minutes, etc. Then click on the `Generate timetable` button to generate the schedule for this period. Verify if the schedules are set to be in seconds or minutes and if need be select whether to allow second-based timetables or keep minute-based.

![Lesson 6: edit timetable](images/lesson6_editTimetable.png)

Once every period has been update, click on the `Save timetable` button ![save button](images/buttonSave.png) to save the current timetable.

Then close the timetable window and save the current line by clicking the line's save ![save button](images/buttonSave.png) button.

### Edit a line to change one end of the path

Similarly to the metro line, we will modify a bus line to completely change one end of its path, to accompany the new metro. In the STM example, we will modify the `141 - Jean-Talon Est`, whose path runs along the metro. We will drop one end and make it serve a nearby hospital from a metro station.

We will edit each path. First to remove the nodes at one end by alt-clicking on the nodes to remove, or by clicking the delete button ![delete button](images/buttonDelete.png) on the bottom panel

![Lesson 6: delete nodes from bottom panel](images/lesson6_deleteFromBottomPanel.png)

Once the nodes have been deleted up to the desired point, new ones can be added, in a similar way than in [the previous metro line edit section](#edit-existing-line-to-add-a-few-stops).

Notice that now, since the line is a bus, it follows the road network.

![Lesson 6: bus line follows road network](images/lesson6_busLineFollowRoadNetwork.png)

Save the path, edit the other direction, then update the schedules as described in the [update schedules section](#update-schedules-for-the-edited-line). 

Before saving the line, we will change its color, so that it is easier to see it in routing panel, as all routes have the same blue color in our network. Save the line.

![Lesson 6: change line color](images/lesson6_changeLineColor.png)

### Validate results with routing

Now that all edits have been done, the data can be prepared for the calculations. To do so, you must save the data to cache for the routing engine (step 2 of the [refresh calculation data section](#refresh-calculation-data)) by clicking on the ![save data to routing cache](images/buttonSaveDataToCache.png). This will automatically restart the routing engine.

You can now go to the `Routing` or `Accessibility maps` panels, select the first scenario that was created in [lesson 2](#lesson-2-prepare-imported-data-for-calculations) and calculate paths that should use the edited lines to see if the data has been properly updated. We see that the first route returned uses our extended metro line and the modified bus.

![Lesson 6: Routing with edited lines](images/lesson6_routingWithEditedLines.png)

*Note*: In this lesson, we have modified existing lines and schedules. We have lost their original data. We could have copied each individual line with the duplicate ![duplicate button](images/buttonDuplicate.png) button, which would have created a new service for each copied line. We also should have edited the scenario, to deactivate the previous lines and add the new lines' services. We could also have duplicated the whole agency, with the duplicate ![duplicate button](images/buttonDuplicate.png) button next to the agency, and edit the lines in the copied agency. In this case, we should have created a new scenario with the copied service of the copied agency.

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
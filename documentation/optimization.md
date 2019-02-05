<span id="title-text">Optimization API Mapping Documentation </span>
==========================================================================================


**bold** = Default Value

*italic* = function

***bold\_italic*** = mapping not possible

`[i] = `Given JSONObject in the JSONArray

`< ... >` = final property that holds value(s)

  

**Mapbox Optimization Response Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>Conversion</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>code</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>Ok</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>waypoints</code></td>
<td>Array&lt;WaypointObject&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><p><br />
</p></td>
</tr>
<tr class="odd">
<td><code>trips</code></td>
<td>Array&lt;TripObject&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

**Waypoint Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>Conversion</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>location</code></td>
<td>Array&lt;Double&gt;</td>
<td><code>solution.routes[i].activities[i].address.&lt;lat&gt;(&lt;lon&gt;)</code></td>
<td>Double</td>
<td><p><em>getLocation()</em></p>
<p>returns the Longitude and Latitude in an Array</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>waypoint_index</code></td>
<td>Integer</td>
<td><code>solution.routes[i].activities</code></td>
<td>JSONArray</td>
<td>Index of  activity used in current waypoint</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>name</code></td>
<td>String</td>
<td><p><code>solution.routes[i].activities[i].&lt;name&gt;</code></p>
<p><code>solution.routes[i].activities[i].&lt;location_id&gt;</code></p></td>
<td>String</td>
<td><p><em>getActivityName()</em></p>
<p>Name of current activity or location_id if name is not defined</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>trips_index</code></td>
<td>Integer</td>
<td><code>solution.routes</code></td>
<td>JSONArray</td>
<td><p>Index of current route</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>


**Trip Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>Conversion</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>distance</code></td>
<td>Integer</td>
<td><code>solution.&lt;distance&gt;</code></td>
<td>Integer</td>
<td>no conversion needed: total distance of trip</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>Integer</td>
<td><code>solution.&lt;transport_time&gt;</code></td>
<td>Integer</td>
<td>no conversion needed: total duration of trip</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>geometry</code></td>
<td>String(polyline)</td>
<td><p><code>(See WaypointObject → location)</code></p>
<p><code>solution.routes[i].&lt;points&gt;</code></p></td>
<td><br />
</td>
<td><p><em>getGeometry()</em></p>
<p>returns polyline6 encoded String of Array of all activity locations or respectively the "points" Array in routes.., if detailed response</p></td>
<td>if calc_points is set to true in configuration object in the problem.json, an array of all visited points along all roads is included in the response. Can be extremely big.</td>
</tr>
<tr class="even">
<td><code>legs</code></td>
<td>Array&lt;LegObject&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

  

**Leg Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>Conversion</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>summary</code></td>
<td>String</td>
<td>(See <code>WaypointObject → name</code>)</td>
<td>String</td>
<td><p><em>getSummary()</em></p>
<p>returns "A to B" (A = current activity name, B= next activity name)</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>steps</code></td>
<td>JSONArray</td>
<td><p><em><strong>no way to request turn-by-turn instructions for GH activities</strong></em></p></td>
<td><br />
</td>
<td><strong>[ ]</strong></td>
<td>Only GH Routing API can return turn-by-turn instructions</td>
</tr>
<tr class="odd">
<td><code>distance</code></td>
<td>Integer</td>
<td><code>solution.routes[i].activities[i].distance</code></td>
<td>Integer</td>
<td><p><em>getActivityDistance()</em></p>
<p>returns accumulated distance of previous activity - acc. distance of current activity</p></td>
<td>waiting_time at an activity is possible in GH and is included in the conversion</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>integer</td>
<td><code>solution.routes[i].activities[i].&lt;arr_time&gt;/&lt;end_time&gt;</code></td>
<td>Integer</td>
<td><p><em>getActivityDuration()</em></p>
<p>returns end_time of previous activity - arrival_time of current activity</p></td>
<td>activity times are timestamps in GH</td>
</tr>
</tbody>
</table>

Ignored Data from the GH Response:

-   `job_id`
-   `status`
-   `waiting_time_in_queue`
-   `processing_time`
-   `unassigned`
-   `solution`
    -   `costs`
    -   `completion_time`
    -   `max_operation_time`
    -   `waiting_time`
    -   `no_vehicles`
    -   `no_unassigned`
    -   `routes[i]`  
        -   `activities[i]`  
            -   `type`
            -   `driving_time`
            -   `load_before`

  

  

Document generated by Confluence on 05.Feb.2019 9:40

[Atlassian](http://www.atlassian.com/)

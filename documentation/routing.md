<span id="title-text">Routing API Mapping Documentation</span>
=====================================================================================


-   **bold** = Default Value
-   *italic* = function
-   ***bold\_italic*** = mapping not possible
-   `[i] = `Given JSONObject in the JSONArray
-   `< ... >` = final property that holds value(s)
-   "Request Parameter" = data that is passed along in a request to the
    proxy, but not used by Graphhopper directly. This information is
    only used in the mappings. Serves the purpose of filling holes of
    information that is not included in a Graphhopper response

  

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
<td><code>routes</code></td>
<td>Array&lt;RouteObject&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
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
<td><code>uuid</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><p><em>getUuid()</em></p>
<p><em><br />
</em>generates unique UUID</p></td>
<td>The usage for this is not clear on Mapbox's side. Seems to have internal purpose</td>
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
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>name</td>
<td>String</td>
<td><p><code>paths[i].instructions[i].&lt;street_name&gt;</code></p>
<p>(here <code>i</code> = either first or last instruction)</p></td>
<td>String</td>
<td><p><em>getFirstStreetName()</em></p>
<p><em>getLastStreetName()<br />
</em></p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>location</code></td>
<td>Array&lt;Double&gt;</td>
<td><code>paths[i].snapped_waypoints.&lt;coordinates&gt;</code></td>
<td>Double</td>
<td>No conversion needed<br />

<p><br />
</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

**Route Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>distance</code></td>
<td>Integer</td>
<td><code>paths[i].&lt;distance&gt;</code></td>
<td>Integer</td>
<td>No conversion needed</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>Integer</td>
<td><code>paths[i].&lt;duration&gt;</code></td>
<td>Integer</td>
<td>divide by 1000</td>
<td><p>ms → s</p></td>
</tr>
<tr class="odd">
<td><code>geometry</code></td>
<td>String (polyline)</td>
<td><code>paths[i].points.&lt;coordinates&gt;</code></td>
<td>Array&lt;Double&gt;</td>
<td><p><em>polyline.encode()</em></p>
<p>returns polyline encoded String of Array</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>weight</code></td>
<td>Integer</td>
<td>(See <code>.duration</code>)</td>
<td>Integer</td>
<td><br />
</td>
<td>It is not clear if the weight property is actually important</td>
</tr>
<tr class="odd">
<td><code>weight_name</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>routability</strong></td>
<td><br />
</td>
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
<tr class="odd">
<td><code>routeOptions</code></td>
<td>RouteOptions Object</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>voiceLocale</code></td>
<td>String</td>
<td>Request Parameter: <code>locale</code></td>
<td>String</td>
<td>No conversion needed</td>
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
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>distance</code></td>
<td>Integer</td>
<td>(See Route Object → <code>distance</code>)</td>
<td><br />
</td>
<td><br />
</td>
<td>As of now, a route contains of only one leg, since only A→ B navigation is supported. A → B → C navigation is not.</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>Integer</td>
<td>(See Route Object → <code>duration</code>)</td>
<td><br />
</td>
<td><br />
</td>
<td>"</td>
</tr>
<tr class="odd">
<td><code>summary</code></td>
<td>String</td>
<td><p>(See Waypoint Object → <code>name</code>)</p>
<p>(First and Last Street Name)</p></td>
<td><br />
</td>
<td><p><em>getSummary()</em></p>
<p>returns "A.street_name to B.street_name"</p>
<p><em>getFirstStreetName()</em></p>
<p><em>getLastStreetName()<br />
</em></p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>steps</code></td>
<td>Array&lt;StepObject&gt;</td>
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

**Step Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>name</code></td>
<td>String</td>
<td><code>paths[i].instructions[i].&lt;street_name&gt;</code></td>
<td>String</td>
<td>No conversion needed</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>Integer</td>
<td><code>paths[i].instructions[i].&lt;time&gt;</code></td>
<td>Integer</td>
<td>&lt;time&gt; / 1000</td>
<td>ms → s</td>
</tr>
<tr class="odd">
<td><code>weight</code></td>
<td>Integer</td>
<td>(See <code>.duration</code>)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>distance</code></td>
<td>Integer</td>
<td><code>paths[i].instructions[i].&lt;distance&gt;</code></td>
<td>Integer</td>
<td>No conversion needed</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>geometry</code></td>
<td>String(polyline)</td>
<td><p><code>paths[i].instructions[i].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><br />
</td>
<td><p><em>polyline.encode( sectionPoints)</em></p>
<p>Returns an encoded polyline String of those points that are part of the instruction interval</p></td>
<td><code>interval</code> saves the indexes for the <code>points</code> array, to look up the coordinates which part of an instruction. </td>
</tr>
<tr class="even">
<td><code>driving_side</code></td>
<td>String</td>
<td><em><strong>No way to get the driving_side, as it is not part of the GH response</strong></em></td>
<td>String</td>
<td><strong>right</strong></td>
<td><p>Workaround would be to detect the country that your navigating in and by that decide the driving_side</p></td>
</tr>
<tr class="odd">
<td><code>mode</code></td>
<td>String</td>
<td>Request parameter: <code>vehicle</code></td>
<td>String</td>
<td><p><em>convertProfile()</em></p>
<p>returns the mode of vehicle (e.g car → driving, foot → walking)</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>maneuever</code></td>
<td>ManeuverObject</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>intersections</code></td>
<td>Array<br />
&lt;IntersectionObject&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>voiceInstructions</code></td>
<td><p>Array<br />
&lt;VoiceInstructionsObject&gt;</p></td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>bannerInstructions</code></td>
<td>Array<br />
&lt;BannerInstructionsObject&gt;</td>
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

  

**Maneuver Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>bearing_before</code></td>
<td>Integer</td>
<td><p><code>paths[i].instructions[i].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><p>Array&lt;Integer&gt;</p>
<p>Array&lt;Double&gt;</p></td>
<td><p><em>getBearingBefore()</em></p>
<p>returns the bearing of the vehicle before a maneuver (= angle between either the coordinates of last intersection or location of current instruction and location of the maneuver)</p>
<p><br />
</p></td>
<td><p>0 &lt;= bearing &lt;= 360°<br />
0° → north</p>
<p>180° → south</p>
<p><br />
</p></td>
</tr>
<tr class="even">
<td><code>bearing_after</code></td>
<td>Integer</td>
<td><p><code>paths[i].instructions[i+1].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><p>Array&lt;Integer&gt;</p>
<p>Array&lt;Double&gt;</p></td>
<td><p><em>getBearingAfter()</em></p>
<p>returns the bearing of the vehicle after a maneuver (= angle between location of maneuver and either the coordinates of first intersection or coordinates of  next maneuver)</p>
<p><br />
</p></td>
<td><p>0 &gt;= bearing &lt;= 360°<br />
0° → north</p>
<p>180° → south</p>
<p><br />
</p></td>
</tr>
<tr class="odd">
<td><code>location</code></td>
<td>Array&lt;Double&gt;</td>
<td><p><code>paths[i].instructions[i].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><p>Array&lt;Integer&gt;</p>
<p>Array&lt;Double&gt;</p></td>
<td>the first index of <code>&lt;interval&gt;</code> used as lookup for <code>&lt;coordinates&gt;</code></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>modifier</code></td>
<td>String</td>
<td><p><code>paths[i].instructions[i].&lt;sign&gt;</code></p></td>
<td>Integer</td>
<td><p><em>getMapboxModifier()</em></p>
<p>converts the Graphhopper <code>&lt;sign&gt;</code> integers to <code>modifier</code> like "sharp left", "right" etc.</p></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>type</code></td>
<td>String</td>
<td><p><code>paths[i].instructions[i].&lt;sign&gt;</code></p></td>
<td>Integer</td>
<td><p><em>getType()</em></p>
<p>converts the Graphhopper <code>&lt;sign&gt;</code> Integers to <code>type</code> s like "arrive", "rounabout" etc.</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>instruction</code></td>
<td>String</td>
<td><p><code>paths[i].instructions[i].&lt;text&gt;</code></p></td>
<td>String</td>
<td>No conversion needed</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><p><code>exit</code></p>
<p><em>(option for</em> <code>type=roundabout</code><em>)</em></p></td>
<td>Integer</td>
<td><p><code>paths[i].instructions[i].&lt;exit_number&gt;</code></p></td>
<td>Integer</td>
<td>No conversion needed</td>
<td>Number of the exit in a roundabout</td>
</tr>
</tbody>
</table>

  

**Intersection Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>out</code></td>
<td>Integer</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>0</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>entry</code></td>
<td>Array&lt;Boolean&gt;</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>[true]</strong></td>
<td><p>See mapbox Doc for Intersection explanation.</p>
<p>The mapper always creates only one "true" way out of an intersection</p></td>
</tr>
<tr class="odd">
<td><code>bearings</code></td>
<td>Array&lt;Integer&gt;</td>
<td><p><code>paths[i].instructions[i].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><p>Array&lt;Integer&gt;</p>
<p>Array&lt;Double&gt;</p></td>
<td><p><em>calculateBearing()</em></p>
<p>returns the angle between this and the next intersection</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>location</code></td>
<td>Array&lt;Double&gt;</td>
<td><p><code>paths[i].instructions[i].&lt;interval&gt;</code></p>
<p><code>paths[i].points.&lt;coordinates&gt;</code></p></td>
<td><p>Array&lt;Integer&gt;</p>
<p>Array&lt;Double&gt;</p></td>
<td>no conversion needed: lookup index of interval in <code>&lt;coordinates&gt;</code></td>
<td><br />
</td>
</tr>
</tbody>
</table>

**Voice Instruction Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>distanceAlongGeometry</code></td>
<td>Integer</td>
<td><br />
</td>
<td>Integer</td>
<td>set arbitrarily and subject to change</td>
<td>Distance to next maneuver at which the voiceInstruction should be announced. This is either FAR=2000m, ... , VERY_CLOSE = 200m</td>
</tr>
<tr class="even">
<td><code>announcement</code></td>
<td>String</td>
<td><code>paths[i].instructions[i].&lt;text&gt;</code></td>
<td>String</td>
<td><br />
</td>
<td>Can also include the next instruction's text, e.g. if the next instruction is a really short turn</td>
</tr>
<tr class="odd">
<td><code>ssmlAnnouncement</code></td>
<td>String</td>
<td><code>paths[i].instructions[i].&lt;text&gt;</code></td>
<td>String</td>
<td><p><em>getSsmlAnnouncement()</em></p>
<p>returns the announcement in ssml syntax</p></td>
<td>Mapbox Navigation will use the ssml announcement if valid</td>
</tr>
</tbody>
</table>

**Banner Instruction Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>distanceAlongGeometry</code></td>
<td>Integer</td>
<td><code>paths[i].instructions[i].&lt;distance&gt;</code></td>
<td>Integer</td>
<td>set arbitrarily and subject to change</td>
<td>Distance to next maneuver at which the voiceInstruction should be announced. This is either FAR=2000m, ... , VERY_CLOSE = 200m</td>
</tr>
<tr class="even">
<td><code>primary</code></td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>primary.text</code></td>
<td>String</td>
<td><code>paths[i].instructions[i+1].&lt;text&gt;</code></td>
<td>String</td>
<td>No conversion needed</td>
<td>The text displayed is the text of the next! instruction, not the current one</td>
</tr>
<tr class="even">
<td><code>primary.type</code></td>
<td>String</td>
<td><code>paths[i].instructions[i+1].&lt;sign&gt;</code></td>
<td>Integer</td>
<td><p><em>getType()</em></p>
<p>(See Maneuver Object → type)</p>
<p><br />
</p></td>
<td>Maneuver type of next instruction</td>
</tr>
<tr class="odd">
<td><code>primary.modifier</code></td>
<td>String</td>
<td><code>paths[i].instructions[i+1].&lt;sign&gt;</code></td>
<td>Integer</td>
<td><p><em>getMapboxModifier()</em></p>
<p>(See Maneuver Object → modifier)</p></td>
<td><p>Maneuver modifier of next instruction</p>
<p><br />
</p></td>
</tr>
<tr class="even">
<td><code>components.text</code></td>
<td>String</td>
<td><code>(See primary.text</code>)<br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>components.type</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>text</strong></td>
<td><br />
</td>
</tr>
</tbody>
</table>

**RouteOptions Object**

<table>
<thead>
<tr class="header">
<th>Mapbox</th>
<th>needed type</th>
<th>Used Graphhopper Data</th>
<th>type</th>
<th>conversion applied</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>baseUrl</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>https://api.mapbox.com</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>user</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>mapbox</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>profile</code></td>
<td>String</td>
<td>Request parameter: <code>locale</code></td>
<td><br />
</td>
<td><p><em>convertProfile()</em></p>
<p>(See Step Object → mode)</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>coordinates</code></td>
<td>Array&lt;Double&gt;</td>
<td><code>paths[i].snapped_waypoints.coordinates</code></td>
<td>Array&lt;Double&gt;</td>
<td>No conversion needed</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>language</code></td>
<td>String</td>
<td>Request parameter:  <code>locale</code></td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>bearings</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>";"</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>continueStraight</code></td>
<td>Boolean</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>true</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>roundaboutExits</code></td>
<td>Boolean</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>true</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>geometries</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>polyline6</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>overview</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>full</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>steps</code></td>
<td>Boolean</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>true</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>annotations</code></td>
<td>String</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>""</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>voiceInstructions</code></td>
<td>Boolean</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>true</strong></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>bannerInstructions</code></td>
<td>Boolean</td>
<td><br />
</td>
<td><br />
</td>
<td><strong>true</strong></td>
<td><br />
</td>
</tr>
<tr class="odd">
<td><code>voiceUnits</code></td>
<td>String</td>
<td>Request parameter:  <code>locale</code></td>
<td>String</td>
<td><p><em>getUnitSystem()</em></p>
<p>returns either "metric" or "imperial" based on given language e.g.  <code>locale =en-us</code> will return "imperial"</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><code>accessToken</code></td>
<td>String</td>
<td>Request parameter: <code>mapboxkey</code></td>
<td>String</td>
<td>no conversion needed</td>
<td>Mapboxkey can be passed along the request</td>
</tr>
<tr class="odd">
<td><code>requestUuid</code></td>
<td>String</td>
<td><p><em><strong>No such data in GH response, has to generated while mapping</strong></em></p></td>
<td>String</td>
<td><em>generateUuid()</em></td>
<td><br />
</td>
</tr>
</tbody>
</table>

Ignored Data from the GH Response:

-   `paths[i]`
    -   `bbox`
    -   `details`

Document generated by Confluence on 05.Feb.2019 9:40

[Atlassian](http://www.atlassian.com/)

Caching
=======

<tt>/api/autosuggest</tt> and <tt>/api/search</tt> will return appropriate HTTP caching headers. Use your HTTP stack's
support for caching to determine the lifetime of caching an object.

For the <tt>My Trip</tt> feature, the app will need to remember the JSON document associated with the user's saved attractions. There is no API to fetch the JSON document for a list of IDs.

For recent searches, remember the JSON document from <tt>/api/autosuggest</tt> for searches that the user selected.

Autosuggest
=========

 `GET /api/autosuggest`

Examples:
* http://tripwhat.com/api/autosuggest?lat=47.609722&lng=-122.333056&q=new
* http://tripwhat.com/api/autosuggest?lat=47.609722&lng=-122.333056&q=space
* http://tripwhat.com/api/autosuggest?lat=47.609722&lng=-122.333056&q=pi
* http://tripwhat.com/api/autosuggest?lat=47.609722&lng=-122.333056&kind=trip

<table>
<tr>
<th>Parameter</th><th>Description</th><th>Examples</th>
</tr>
<td>q</td><td>The search query. (optional)</td><td><tt>Space Needle</tt>, <tt>sushi</tt>, <tt>Seattle</tt></td>
</tr>
<tr><td>lat (optional)</td><td>The user's latitude; will be inferred from IP address if omitted.</td><td><tt>47.609722</tt></td></tr>
<tr><td>lng (optional)</td><td>The user's longitude; will be inferred from IP address if omitted.</td><td><tt>-122.333056</tt></td></tr>
<tr><td>kinds (optional)</td><td>Comma-separated list of the kind of results desired.</td><td><tt>trip</tt></td></tr>
</table>

**App interaction**

If a response has bounds, reset the map bounds to include that region. If a response has an ID, include that ID in the
<tt>ids</tt> parameter of the <tt>/api/search</tt> service to ensure you receive its information.

**Response**

`AutosuggestEntry[]`, where AutosuggestEntry is an object with these fields:

<table>
<tr>
<th>Field</th><th>Description</th><th>Examples</th>
</tr>
<tr><td>id (optional)</td><td>String. Only returned for attractions (e.g., the Space Needle); will not be present for queries like <tt>sushi restaurant</tt> or <tt>seattle</tt>.</td><td><tt>-9223372036852184203</tt></td></tr>
<tr><td>name</td><td>String. The name of the suggested query.</td><td><tt>sushi restauraunts</tt>, <tt>Seattle</tt>, <tt>Space Needle</tt></td></tr>
<tr><td>location (optional)</td><td>String. The location of the attraction.</td><td><tt>Seattle, WA</tt></td></tr>
<tr><td>kind</td><td>String. The kind of attraction.</td><td><tt>city</tt>, <tt>hood</tt>, <tt>sight</tt>, <tt>food</tt>, <tt>event</tt>, <tt>query</tt></td></tr>
<tr><td>latlng (optional)</td><td><tt>LatLng</tt> object. The position of the item.</td><td>See <tt><a href="#latlng">LatLng</a></tt>.</td></tr>
<tr><td>bounds (optional)</td><td><tt>GeoBounds</tt> object. The recommended viewport for the map if the user selects this item.</td><td>See <tt><a href="#geobounds">GeoBounds</a></tt>.</td></tr>
</table>

If `bounds` is returned, the app should refocus the map such that it contains those bounds.

If `id` is returned, the app should highlight that marker when it's returned from a subsequent `/api/search` request.

Search Results
=========
`GET /api/search`

For a given bounding geographical area, return attractions that match a query or a filter.

Examples:
* http://tripwhat.com/api/search?south=47.594739&east=-122.307658&north=47.646229&west=-122.391772&kinds=sight,food,event
* http://tripwhat.com/api/search?south=47.594739&east=-122.307658&north=47.646229&west=-122.391772&kinds=event&from=2013-01-10T10:00:00Z&to=2013-02-10T10:00:00Z
* http://tripwhat.com/api/search?south=47.594739&east=-122.307658&north=47.646229&west=-122.391772&kinds=sight,food,event&ids=-9223372036852154777
* http://tripwhat.com/api/search?south=47.594739&east=-122.307658&north=47.646229&west=-122.391772&kinds=trip&trips=-9223372036849649407

**App interaction**

When panning the map or zooming into the map, include new results and leave the old results on the map. When zooming out the map, reset the results
and only show the freshest results.

<table>
<tr>
<th>Parameter</th><th>Description</th><th>Examples</th>
</tr>
<td>q (optional)</td><td>The search query.</td><td><tt>sushi</tt>, <tt>museum</tt></td>
</tr>
<tr><td>north</td><td>The northern latitude.</td><td><tt>47.646229</tt></td></tr>
<tr><td>east</td><td>The eastern longitude.</td><td><tt>-122.307658</tt></td></tr>
<tr><td>south</td><td>The southern latitude.</td><td><tt>47.594739</tt></td></tr>
<tr><td>west</td><td>The western longitude.</td><td><tt>-122.391772</tt></td></tr>
<tr><td>kinds</td><td>Comma-separated string of attraction kinds to return.</td><td><tt>sight,food,event,trip</tt></td></tr>
<tr><td>trips</td><td>Comma-separated string of trips to return.</td><td><tt>-9223372036849649407</tt></td></tr>
<tr><td>keys</td><td>Comma-separated string of trip keys.</td><td><tt>nh-cbioxwmm5</tt></td></tr>
<tr><td>from (optional)</td><td>An ISO 8601 timestamp; events must be scheduled AFTER this value. If not present, defaults to the current time.</td><td><tt>2013-01-10T10:00:00Z</tt></td></tr>
<tr><td>to (optional)</td><td>An ISO 8601 timestamp; events must be scheduled BEFORE this value. If not present, defaults to the current time plus 24 hours.</td><td><tt>2013-01-11T10:00:00Z</tt></td></tr>
<tr><td>ids</td><td>Comma-separated list of attraction IDs. These IDs are guaranteed to be in the response document.</td><td><tt>-9223372036852184203</tt></td></tr>
</table>

**Error Response**

If the request is invalid, you will get an object with these fields:

<table>
<tr>
<th>Field</th><th>Description</th><th>Examples</th>
</tr>
<tr><td>status</td><td>String.</td><td><tt>error</tt></td></tr>
<tr><td>message</td><td>String. An explanation of the problem. Do not show this to end-users;</td><td><tt>invalid parameter: when (could not parse 'foo')</tt></td></tr>
</table>

**Response**

<tt>SearchResponse</tt>, an object with these fields:

<table>
<tr>
<th>Field</th><th>Description</th><th>Examples</th>
</tr>
<tr><td>status</td><td>String.</td><td><tt>ok</tt></td></tr>
<tr><td>num_sight</td><td>Integer. The number of attractions (e.g. the Space Needle) in the viewport; including those not explicitly listed in <tt>results</tt>.</td><td><tt>128</tt></td></tr>
<tr><td>num_food</td><td>Integer. The number of restaurants in the viewport; including those not explicitly listed in <tt>results</tt>.</td><td><tt>20</tt></td></tr>
<tr><td>num_event</td><td>Integer. The number of events in the viewport; including those not explicitly listed in <tt>results</tt>.</td><td><tt>10</tt></td></tr>
<tr><td>results</td><td><tt>TravelAttraction[]</tt>. The most highly-recommended attractions that met the search criteria.</td><td>See <tt><a href="#travelattraction">TravelAttraction</a></tt>.</td></tr>
</table>

Other Types
=========
<a name="geobounds"/><tt>GeoBounds</tt> represents a recommended viewport for the map. It is an object with these fields:

<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>north</td><td>Double. The northern latitude.</td><td><tt>47.646229</tt></td></tr>
<tr><td>east</td><td>Double. The eastern longitude.</td><td><tt>-122.307658</tt></td></tr>
<tr><td>south</td><td>Double. The southern latitude.</td><td><tt>47.594739</tt></td></tr>
<tr><td>west</td><td>Double. The western longitude.</td><td><tt>-122.391772</tt></td></tr>
</table>

<a name="latlng"/><tt>LatLng</tt> represents the location of the item. It is an object with these fields:

<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>lat</td><td>Double. The latitude.</td><td><tt>47.646229</tt></td></tr>
<tr><td>lng</td><td>Double. The longitude.</td><td><tt>-122.307658</tt></td></tr>
</table>


<a name="travelattraction"/><tt>TravelAttraction</tt> represents a generic attraction (a restauraunt, a concert, or a tourist sight like the Statue of Liberty). It is an object with these fields:

<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>id</td><td>String. The ID of this attraction.</td><td><tt>-9223372036852184203</tt></td></tr>
<tr><td>kind</td><td>String. The kind of the attraction.</td><td><tt>sight</tt>, <tt>food</tt>, or <tt>event</tt></td></tr>
<tr><td>name</td><td>String. The name of the attraction.</td><td><tt>Space Needle</tt></td></tr>
<tr><td>lat</td><td>Double. The latitude of the attraction.</td><td><tt>47.620484</tt></td></tr>
<tr><td>lng</td><td>Double. The longitude of the attraction.</td><td><tt>-122.349715</tt></td></tr>
<tr><td>thumbs</td><td><tt>Photo[]</tt>. Thumbnails of the attraction; at most 100x100. May be empty.</td><td>See <tt><a href="#photo">Photo</a></tt>.</td></tr>
<tr><td>photos</td><td><tt>Photo[]</tt>. Photos of the attraction; at most 640x480. May be empty.</td><td>See <tt><a href="#photo">Photo</a></tt>.</td></tr>
<tr><td>address (optional)</td><td>String. The address of the attraction.</td><td><tt>400 Broad St</tt></td></tr>
<tr><td>city (optional)</td><td>String. The city of the attraction.</td><td><tt>Seattle</tt></td></tr>
<tr><td>phone (optional)</td><td>String. The phone number of the attraction.</td><td><tt>(206) 905-2111</tt></td></tr>
<tr><td>twitter (optional)</td><td>String. The twitter handle of the attraction.</td><td><tt>space_needle</tt></td></tr>
<tr><td>facebook_url (optional)</td><td>String. The URL of the Facebook page of the attraction.</td><td><tt>http://www.facebook.com/spaceneedle/info</tt></td></tr>
<tr><td>wikipedia_url (optional)</td><td>String. The URL of the Wikipedia page of the attraction.</td><td><tt>http://en.wikipedia.org/wiki/Space_Needle</tt></td></tr>
<tr><td>description</td><td>String. A short description of the attraction.</td><td><tt><tt>
  The Space Needle is a tower in Seattle, Washington and a major landmark of the Pacific Northwest region of the United
  States and a symbol of Seattle.</tt></td></tr>
<tr><td>reviews (optional)</td><td>Map[String, Review]. A map of reviews from popular sites.</td><td>Keys:
<tt>urbanspoon</tt>, <tt>yelp</tt>. Values: See <tt><a href="#review">Review</a></tt>.</td></tr>
<tr><td>url (optional)</td><td>String. The URL of the attraction's website.</td><td><tt>http://spaceneedle.com/</tt></td></tr>
<tr><td>hours_today (optional)</td><td>String. The hours of the attraction today.</td><td><tt>10am - 11:30pm</tt>, <tt>Closed</tt></td></tr>
<tr><td>price_range (optional)</td><td>String. The general price range of the attraction.</td><td><tt>$</tt>, <tt>$$</tt>, <tt>$$$</tt></td></tr>
<tr><td>subkind (optional)</td><td>String. The specific kind of attraction.</td><td><tt>Musical</tt>, <tt>Pro Sports</tt>, <tt>French</tt>. Not an exhausitive list.</td></tr>
<tr><td>showtimes (optional)</td><td><tt>Showtime[]</tt>. The list of showtimes and prices.</td><td>See <tt><a href="#showtime">Showtime</a></tt>.</td></tr>
</table>

<a name="review"/><tt>Review</tt> represents a link to an external review of an attraction. It is an object with these fields:

<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>rating (optional)</td><td>Double. The restaurant's rating on Urban Spoon.</td><td><tt>4.5</tt></td></tr>
<tr><td>reviews (optional)</td><td>Int. The number of reviews of the restaurant on Urban Spoon.</td><td><tt>128</tt></td></tr>
<tr><td>url (optional)</td><td>String. The URL of the restaurant's Urban Spoon page.</td><td><tt>http://www.urbanspoon.com/r/1/482/restaurant/Westlake/Canlis-Seattle</tt></td></tr>
</table>


<a name="photo"/><tt>Photo</tt> represents a photo of an attraction. It is an object with these fields:

<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>url</td><td>String. The URL of the image.</td><td><tt>http://s3.sortable-static.com/img/travel/item:-9223372036852184203:photos/fe71640ecda0086fc16365bddd834e46-75-100</tt></td></tr>
<tr><td>w</td><td>Int. The width of the image in pixels.</td><td><tt>75</tt></td></tr>
<tr><td>h</td><td>Int. The height of the image in pixels.</td><td><tt>100</tt></td>
</tr>
<tr><td>credit (optional)</td><td>The source of the URL.</td><td><tt>Wikipedia</tt></td></tr>
<tr><td>credit_url (optional)</td><td>A link to the source of the URL.</td><td><tt>http://en.wikipedia.org/wiki/File:Space_Needle_2011-07-04.jpg</tt></td></tr>
<tr><td>caption (optional)</td><td>A caption for the image.</td><td><tt>The Space Needle, as seen from below.</tt></td></tr>
</table>

<a name="showtime"/><tt>Showtime</tt> represents a showtime and a price range for tickets. It is an object with these fields:


<table>
<tr><th>Field</th><th>Description</th><th>Examples</th></tr>
<tr><td>time</td><td>String. An ISO8601 time of when the event is happening.</td><td><tt>2013-01-10T10:00:00Z</tt></td></tr>
<tr><td>min (optional)</td><td>Double. The price of the cheapest ticket.</td><td><tt>35</tt></td></tr>
<tr><td>max (optional)</td><td>Double. The price of the most expensive ticket.</td><td><tt>100</tt></td>
</tr>
</table>

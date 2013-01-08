# Overview #
TripWhat is a mobile app to help people plan a trip.  

## Map screen ##
My location
* Pushing the 'my location' symbol, the map should center around the user's location, similar to google maps

Search box
* Touching the search box brings up the search page, similar to google maps

My Trip Icon
* This is the blue asterisk in the search box
* Touching this invokes the "My trip" search

Pins
* Pins are color coded to match their symbol
* Clicking a pin selects that pin and then shows details about that pin at the bottom, see Map with Detail

Detail panel
* This panel is shown instead of the results panel when a pin has been selected

Results panel
* This is the panel at the bottom
* It represents the places found within the map bounds
* It has three icons: events, attractions and restaurants
* Beside each icon it has a number indicating how many places of that type are in the map bounds
* Touching one of these icons filters the visible items to that type, they are multi-select, so you could touch events and restaurants
* Touching List takes you to the List screen
* Dragging the filter handle up reveals another filter: the date filter

Date filter
* This lets the user set what date range they are planning their trip for
* It defaults to Today
* The user can change it to include say next weekend, or the start and end of a future trip
* Its main purpose is to restrict which events are shown, e.g. maybe you're in town today and want to see concerts 

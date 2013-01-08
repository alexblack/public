# Overview #
TripWhat is a mobile app to help people plan a trip.  

## Map screen ##
This is the screen we show to the user when they open the app.

My location
* Pushing the 'my location' symbol, the map should center around the user's location, similar to google maps

Search box
* Touching the search box brings up the search page, similar to google maps (Transition: fade, like google maps)
* The search box displays the current search if there is one
* When there is a current search the X button is displayed to the right of the search box so the search can be cleared

My Trip Icon
* This is the blue asterisk in the search box
* Touching this invokes the "My trip" search

Pins
* Pins are color coded to match their symbol
* Touching a pin selects that pin and then shows the Detali panel about that pin at the bottom, see Map with Detail (transition proposal: vertically slide the results panel down)
* Touching a pin will "slide" the current place out of the details panel and be replaced by the place of the new pin (assuming the Detail panel is visible)
* Touching the map (not on a pin) deselects the current pin, and hides the Detail panel (transition proposal: vertically slide the results panel back up)
* Pins for places that have been saved to your trip need to be distinguished visually, with a star perhaps

Detail panel
* This panel is shown instead of the results panel when a pin has been selected
* If the user deselects a pin (by clicking on the map) then this is hidden (revealing the results panel)
* The detail panel can be swiped horizontally to scroll through the places
* When the detail panel is swiped the old pin should be deselected and the new pin selected, so the user can see where this place is on the map (transition: pins should grow/shrink like google maps)
* Touching the detail panel takes you to the Detail screen (transition: slide up)

Results panel
* This is the panel at the bottom
* It represents the places found within the map bounds
* It has three icons: events, attractions and restaurants
* Beside each icon it has a number indicating how many places of that type are in the map bounds
* The filter numbers should update in realtime when the map is zoomed or panned, or when the search changes
* Touching one of these icons filters the visible items to that type, they are multi-select, so you could touch events and restaurants
* Touching List takes you to the List screen
* Dragging the filter handle up reveals another filter: the date filter

Date filter
* This lets the user set what date range they are planning their trip for
* It defaults to Today
* The user can change it to include say next weekend, or the start and end of a future trip
* Its main purpose is to restrict which events are shown, e.g. maybe you're in town today and want to see concerts 

Zooming and panning the map
* This changes the bounds of the user's search
* The # of results of each type shoudl update in realtime in the Results panel

## Detail screen ##

This screen is used to learn more about one of the attractions

Navigation
* The list button takes you back to the list
* The user can swipe horizontally to go through the list (and get from end to beginning)

Layout
* There are slightly different templates for each type of attraction: event, restaurant or attraction
* For restaurants we want to show the hours it is open today and the price range, for events we want to show the showtimes and price range

Buttons
* The call button invokes the phone dialer and should be shown if we have the phone number
* The directions button invokes Navigation in google maps from current location to this location
* The website button opens the website in the browser
* Yelp button opens the yelp webpage in the browser
* The tickets button opens the seat geak page in the browser
* More [Description] - this button reveals more of the description
* More photos - this reveals the rest of the photos

## Search Screen ##

This should be quite similar to Google Maps on IOS.  

Two types of searches
* Locations like New York which will move the map to that location and show you places in New York
* Types of places like "steak houses" or "fun" which will search the current results for places that match that text

Navigation
* Pusing cancel takes you back to the map screen

Empty search
* When the search screen is first displayed the user won't have a search typed in yet
* It should show a "special" search at the top of the recent searches: "My trip"
* It should show the users recent searches
* Touching a recent search executes that search and takes you to the Map page
* Touching "My trip" takes you to the map page with the "My trip" search executed, e.g. shows you your saved places

Autocomplete
* As the user types in a search it will show them suggestions
* Some suggestions will be locations that move the map (like New York) others will be keywords that search for places on the current map (e.g. steak houses)

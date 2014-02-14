# End users screens (Mobile)

## Listings screen
This screen lets the user scroll through listings.
- It should show 10 listings right away, when the user scrolls to the bottom of those there should be a visual indicator to let them know more results are loading in
- Avoid breaking the back button with infinite scroll, see https://medium.com/design-ux/51b224e42926
- Tapping 'popular' reveals the category menu
- When the user changes category the current category is displayed where 'Popular' is, and results are reloaded
- Tapping 'filer' reveals the filter screen
- Tapping the star on any listing adds (or removes) that listing from the list of saved items
- Tapping a listing takes you to the listing details screen

## Listing details screen
This screen lets you read the information about the listing and swipe through the photos
- Swiping to the left will advance to the next photo
- Tapping the star adds (or removes) the listing from the list of saved items
- Tapping the share icon prompts you to share the listing page url on facebook, twitter, email
- Tapping the buy button takes you to the original url of the listing (say on craigslist)
 - *important*: Open this url in a new tab (so we can easily log the buy event to google analytics)
- Tapping the photo hides all other UI elements, swiping is still possible, tapping again reveals the UI elements
- Pushing browser-back at any point takes you to the listings screen (not to a previous photo)

## Displaying results
- Results should be shown within 25 miles of the user's location, by default
- The site should use the user's location determined by GPS (if user permits) and fallback to geoip location
- Results should be sorted by popularity, popularity is defined as # of listing-detail pageviews in the last 30 days + # of times the listing has been saved * 10 in the last 30 days

## Url proposals
- http://hostname.com/popular (displays popular listings)
- http://hostname.com/wedding (displays listings in category wedding)
- http://hostname.com/wedding?max-price=250 (listings for less than $250)
- http://hostname.com/saved (displays starred listings)
- http://hostname.com/1244-red-dress (displays listing with id 1244)

# Admin screen (Desktop)

## Add listing screen
This screen lets the user add a listing from Craigslist.
- The user pastes a url from a craigslist listing
- The form is then popularted from the craigslist listing
- The user can then change the desc, specify a category etc and hit ADD
- The listing should then be added to the database, and the user given a 302 redirect to the listing details screen

# Google Analytics
Its important we understand what users are interested in and what they do on the site. 
## Pageviews
- Pageviews for the listings screen and listing details screen should be logged like normal to google analytics
- When the user uses infinite scroll a pageview should be logged to google analytics like http://hostname.com/wedding/2, or http://hostname.com/shoes/10 for example, so if the user scrolls to see 100 items then 10 pageviews will be tracked total
## Events
Events should be logged with (category, action, label)
- When the user stars or unstars a listing on listings or details screen, Category=listing, Action=star/unstar, label=<listing-url>
- When the user clicks buy on a listing, Category=listing, Action=buy, label=<original-url>
- When the user clicks the category menu, Category=Nav, Action=change-category
- When the user clicks the filter menu, Category=Nav, Ation=filter
- When the user clicks the post-listing-for-sale, Category=Nav, Action=post
- When the user clicks the share button, Category=listing, Action=share

# Proposed database

## Table: Listing
- id
- ext_url - url on 3rd party site
- price
- date_posted
- date_inactive
- caption
- description
- location (city/region, postal code, ?)
- seller_name
- category_id

## Table: Categories
- id
- caption

## Table: Photos
- id
- is_primary
- url
- listing_id


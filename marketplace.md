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
- Tapping the photo hides all other UI elements, swiping is still possible, tapping again reveals the UI elements
- Pushing browser-back at any point takes you to the listings screen (not to a previous photo)

# Admin screens (Desktop)

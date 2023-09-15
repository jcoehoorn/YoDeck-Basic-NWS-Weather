# Basic NWS Weather for YoDeck

This shows current conditions and the forecast for three days (including the current day).

***Note: This is still very much a work-in-progres.***  
Use at your own risk.

It is not as customizable as the default YoDeck weather app, but it's _MUCH_ faster to load, and seems to have more-reliable forecasts.

When adding this app, it should be configured to use Chromium to render properly and as a Semi-static app to avoid hitting NWS API limits.

To get local data, the NWS API needs four separate pieces of the information:

1. The Office ID for your local NWS Office
2. The grid coordinates within that office's area of responsibility
3. The Weather Station ID for the nearest weather station
4. The Radar Station ID for the nearest radar service.

To find this information, start from Google Maps and right-click on your location. This will show you lattitude and longitude coordinates, and if you click them it will copy them to the clipboard. From here, you can visit the following address in a browser:

https://api.weather.gov/points/{lattitude,longitude_from_google}

_(Google's coordinates will have a space between Lat and Long that must be removed.)_

Do this correctly and you will see a bunch of text in the browser in JSON format which includes values like this:

    "gridId": "AAA",
    "gridX": 01,
    "gridY": 02,
	
The actual value from gridID is the Office ID needed for the App, and gridX and gridY are the grid coordinates (entered as 01,02).

To find the station, load a similar page at the following address using the data just collected:

https://api.weather.gov/gridpoints/{GID}/{x},{y}/stations

Look for JSON properties with the form `"stationIdentifier": "AAAA"`. The "AAAA" code is the station ID. You should have several of these in the results, but there is also a `name` property that will help you choose the most appropriate option.

For the Radar Station ID, start at the main NWS web site and search for your location.
On the page for your area, there will be a link for your local forcast office.
On the page for your local forecast office there will be a `Radar` link that shows a menu when you hover with the mouse.
Click the `Local Standard Radar (low bandwidth)` option. The page the loads will show the Radar Station ID in the address bar.

This was built to mimic a specific prior layout, but does allow for some customization by exposing a configuration option to provide your own CSS styles.
It's also a good idea to use this as just one item at the bottom of a larger layout, to include things like a location label or company logo.

Finally, please note when testing that YoDecks seems to not correctly call init_widge() with the configured items in a basic web preview. You have to actually show this on a real monitor/player to see what it really looks like.

# Spotify Companion
#### Video Demo:  https://youtu.be/S4ugKEqWZoI
#### Description:

This app makes the use of spotify api, to fetch the current users currently playing songs and its lyrics (which is now a spotify premium only feature), the users playlists, the tracks within the playlists and the ability to search songs.

### layout.html
The layout.html is used for reducing code repetition. It implements the header nav bar which uses many css classes provided from bootstrap, as well as my own customization. Bootstrap was used to implement a mobile-friendly header navbar. Jinja syntax was used for displaying the currently &quot;active&quot; option in the navbar. Jinja syntax was also used to extend this layout.html with other html pages.

### Index page
It is the first page(landing page) that you will be directed to, if already logged in. The index page displays the song that you are currently playing on spotify. With the help of &lt;table&gt; tag in html and css, the page has been made visually appealing, with a huge track image towards the left and a brief song description including the title, artist names, and album name just beside it. Different level of &lt;h&gt; tags were used for the brief description. Below this we can see the lyrics of the particular song, these lyrics are fetched with the help of genius api. The track name and artist name were taken from spotify api and simple command, serach_song() was used to get the lyrics from genius api. A function called get_lyrics() in functions.py was used to search for lyrics by trying all possible artists, who featured in the song, using a for loop. If no lyrics were found, it returned &apos;Lyrics not found&apos;. A function called clean_string(), which uses regex, was used to strip the track and artist names of any special characters, to get the best possible match and accurate search results. The index page also consists of a search bar at the very top, which lets you search for songs and the results are provided from spotify api.

### Songs page
With the help of .search() function, provided by spotify API, we can search for different songs of all genres and languages. We get a maximum of 10 results, which also includes similar tracks to the searched song. Spotify API returns a list of dictionaries containing song information, in our backend, we use a for loop to traverse through each dictionary and extract only the required information and append it to another list, which is sent the songs.html through render_template function provided by flask. In songs.html we use jinja's for loop and html table to display each song, made visually appealing with css. There is also a button to add the song to only our own created playlists. Once clicked, we display a modal, which has a dropdown menu to show our playlists and a add button. When we click the button, it will submit the form, if the song doesn't exist in the playlist. With the help of javascript, we check if the song exists, if not a notification is displayed to notify you that you have added the song to the playlist. But if the song exists, we display another modal which contains an &apos;add anyways&apos; and a &apos;cancel&apos; button. 

### Song page
If you click on the songs in songs page, you are redirected to another page, displaying more information about the song, such as album name, song features, the release date and the lyrics. The page is similarly structed to the index page. You can add the particular song to your playlists from this page aswell and it works in the exact same way it worked in the songs page.

### Playlists page
This page displays the current uses saved playlists. Html tables and CSS have been used for formating it. The page gives a brief description of the playlist beside the playlist image. If the playlist doesn't have an image from spotify, a default image is used. On clicking any playlist, we are redirected to a different page, playlist_tracks.html.

### Playlist tracks page
After clicking a playlist in playlists page, we are redirected to this page, which displays all the songs in that particular playlist. Beside each song there is a delete button, which simply deletes the track from the playlist. This delete button is only seen in the playlists created by the user and not for the ones created by different users. In this page there are several sorting algorithms implemented with the help of javascript. In javascript, first we keep a track of the original order in the global scope with the help of saveOrginalOrder() function, which is called after the whole page is loaded, that is window.onload calls this function. defaultSort() function will display the original order with the help of orginalOrder array defined in the global scope and filled with saveOriginalOrder() function. alphaSort() is used for sorting the songs in A-Z order and popSort() is used to sort from most popular song to least using &apos;popularity&apos; from list of tracks dictionary provided by spotify API. Both popSort() and alphaSort() are implemented using sort function with a comparator provided by javascript. The comparator, however, is different for both functions and is defined by us rather than using a default comparator. For the reverse of the above functions, the logic remains the same, only the comparator order of operands are interchanged.

### app.py
app.py is a flask server for handling the backend part. It is first used for authorization of the user in the login route by calling the spotifyOAuth() function defined in functions.py. The purpose of this function is to authorize the user, using the client id and client secret and then with the help od redirect uri, the callback route is invoked. The callback route grants the user's session an access token using spotify API's functions and then redirects the user to index.html. There's a get_token() function to provide the token info from user's session. If the token is expired, it grants the user's session a new(refresh) token using spotify API's functions. All other routes are used to render the html pages(templates). Each route will fetch different information from spotify API's functions, which will return either a dictionary or a list of dictionaries, containing information, for example, current_user_playlists() will return a list of dictionaries, with each dictionary containing information about the playlist, such as playlist name, description, image, etc. There are routes to handle post methods such as adding to playlist and deleting from playlist, which will taken information from the form, such track uri of the track you want to add or delete and the playlist id of the playlist you want to perform these operations on, then their respective functions are called which is provided by spotify's API.

### functions.py
It provides us with a set of &apos;helper functions&apos; in order to reduce repeated code. It also defines a decorated function called login_required, which is used for every route in app.py, to ensure that you are not allowed to access any page without logging in.

### .env
This file is used to store secret credentials provided by spotify and genius APIs.
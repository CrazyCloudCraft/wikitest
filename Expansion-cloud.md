## About
PlaceholderAPI uses an expansion-cloud (A website that has all kinds of expansions stored), to download jar-files, that contain the placeholders for it to use.

The expansion-cloud can be seen under https://api.extendedclip.com/all

## How it works
When you run `/papi ecloud download <expansion name>` will PlaceholderAPI connect to the site, to first check, if that expansion exist. If it does, then the plugin will start to download the latest jar-file from it.  
It also connects and uses the site, whenever you start the server or list the expansions available or installed.

## Adding your own expansion
You can add your own expansion to the expansion-cloud for others to use.  
In order to do that, you have to follow those steps:
1. Make sure you have created a seperate jar-file like explained on the page [[Making an own expansion]].
2. Create an account on the site, or log in, if you already have one.
3. Click on `Expansions` and then on [`Upload New`](https://api.extendedclip.com/manage/add/).
4. Fill out the required information. `Source URL` and `Dependency URL` are optional.
5. Click on the button that says `Choose an file...` and select the jar of your expansion.
    * **Important**! Make sure, that the name of the jar-file contains the same version like you set in the version-field!
6. Click on `Submit Expansion`

Your expansion is now uploaded and will be reviewed by a moderator.  
If everything is ok will your expansion be approved and will be available on the ecloud for PlaceholderAPI*.

> *The expansion can be still downloaded, no matter if it is approved or not. This is only a security-method for PlaceholderAPI, to make sure it doesn't download mallicious files.

## Updating your expansion
Before you update, please note the following:  
If you aren't a verified dev and you upload an update, your expansion will become **unverified** until a moderator reviews the update and approves your update!  
It is recommended to only update the expansion, if it contains huge changes or bug fixes.

To update your expansion, you first have to go to the list of [your expansions](https://api.extendedclip.com/manage/).  
For that click on `Expansions` and select `Your Expansions`.  
After that, follow those steps:
1. Click the name of the expansion, that you want to download.
2. Click on the button that says `Version`
3. Click on `Add Version`
4. Fill out the fields and upload the new jar.
    * **Important**! Make sure, that the name of the jar-file contains the same version like you set in the version-field!
5. Click on `Save Changes`

If you're a verified dev, your version will be approved and is available directly.  
If you aren't a verified dev, you have to wait until a moderator approves the update.

## Downloading a specific expansion version
In some cases you may want to use a specific, older version of an expansion. (for example if you use an old server version and the newest expansion isn't compatible with it)  
For that case is there a way, to download a specific version of an expansion. You can download the expansion either manually, or through PlaceholderAPI itself.  
Here is how you can do it for each.

### Download manually
To download an expansion manually, you first have to connect to the website and go to the expansion of your choice.  
There, you click on the button that says `Version` and click on the download-icon of the version you want to download.

Finally, stop your server, upload the jar to the folder in `/plugins/PlaceholderAPI/expansions` (Make sure to delete the old jar, if there's already one) and start the server again.

### Download with PlaceholderAPI
Run the following command ingame or in your console:  
`/papi ecloud download <expansion> [version]`

To find out, what versions are available for an expansion, run `/papi ecloud info <expansion>`

After you downloaded the specific version, run `/papi reload` to refresh the expansions.
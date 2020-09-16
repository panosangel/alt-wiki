# Firefox Settings #

Version: 57.0.4 (64bit)
OS: Linux Mint 18.2
Date: 2018.01.11

**Read more:** [Firefox Tweak Guide](http://www.tweakguides.com/Firefox_1.html)  
**Source:** [How To Optimize Firefox By Tweaking Hidden Settings In The "about:config" Page](https://www.download3k.com/articles/How-To-Optimize-Firefox-By-Tweaking-Hidden-Settings-In-The-about-config-Page-01955)  
**Source:** [Speed up Firefox with RAM cache and tmpfs in Linux](https://www.pcsuggest.com/speed-up-firefox-with-ram-cache-and-tmpfs-linux/)  
**Source:** [Firefox/Tweaks](https://wiki.archlinux.org/index.php/Firefox/Tweaks)  
**Source:** [Firefox - How to change the location of the temporary files folder](http://ccm.net/faq/40745-firefox-how-to-change-the-location-of-the-temporary-files-folder)  

## Longer interval to save session

Directive: `browser.sessionstore.interval`  
Type: Integer  
Default: 15000 [ms]  
Suggested = 60000  

## Cache Optimization

You can check up on your memory cache activity by typing `about:cache`.

Directive: `browser.cache.use_new_backend`  
Type: Integer  
Default: 0  
Suggested: 1

Directive: `browser.cache.disk.metadata_memory_limit`  
Type: Integer  
Default: 250 [kb]  
Suggested: 51200

Directive: `browser.cache.disk.parent_directory`  
Type: String  
Default: NA  
Suggested: /tmp  

## Network

`Pipelining` is not supported in Firefox 57+
**Source** [What happened to pipelining in about:config?](https://support.mozilla.org/en-US/questions/1166780)

Directive: `network.http.max-connections`  
Type: Integer  
Default: 900  
Suggested: 1500

Directive: `network.http.max-persistent-connections-per-server`  
Type: Integer  
Default: 6  
Suggested: 10

## Defragment the profile's SQLite databases

To access this tool, open the `about:support` page, search for `Places Database` and click the `Verify Integrity` button.

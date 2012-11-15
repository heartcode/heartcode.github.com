---
layout: post
title: "Fixing the trailing slash issue in the invalid Chrome extension manifest file"
date: 2012-11-15 23:45
comments: true
categories: JavaScript refactoring Chrome extension
---
<a href="/blog/2012/11/15/fixing-the-trailing-slash-issue-in-the-invalid-chrome-extension-manifest-file/">{% img /images/posts/fixing-the-trailing-slash-issue-in-the-invalid-chrome-extension-manifest-file/fixing-the-trailing-slash-issue-in-the-invalid-chrome-extension-manifest-file.png 789 443 'Trailing slash' 'Trailing slash' %}</a>

The other day I had some spare time and I thought it was a great opportunity to do some JavaScript code refactoring on my [Fontier Chrome extension](https://chrome.google.com/webstore/detail/fontier/dkbamaalakfhckcidgiigdinhcncaeae). The refactoring process didn’t take a long time, because the application itself is very simple. I tested the new version of the add-on in the newest **Chrome (Version 23.0.1271.64)** and in **Canary (Version 25.0.1325.0 canary)** and after seeing that it worked fine in both, I decided to update the extension in the Chrome Web Store.

After I finished refactoring my extension code I logged in to my Developer Dashboard and I updated the extension using the ZIP upload tool. When I was done with the update I removed the 0.3 version of Fontier from my Chrome and I went to the Chrome Store to install the new version. For some reason Chrome didn't let me install the extension :)

When I hit the 'Add to Chrome' button, the following error message popped up under the address bar:

>"Package is invalid. Details: 'Illegal path (absolute or relative with '..'): '/img/icon_48.png".

##But that worked before...

Fair enough, I thought I broke the **manifest.json** file. I was sure that apart from increasing the 'version' from '0.3' to '0.4' I didn't change anything, but I quickly scanned through the manifest to make sure everything was alright. Esentially everything was the same as it was before:

	{
	  "name": "Fontier",
	  "version" : "0.4",
	  "minimum_chrome_version": "22.0.1229.79",
	  "manifest_version": 2,
	  "description" : "The quickest way to change the default font size in Chrome. Great for testing Responsive websites using em and percentage units.",
	  "permissions": ["fontSettings"],
	  "icons": {
	    "16": "/img/icon_16.png",
	    "48": "/img/icon_48.png",
	    "128": "/img/icon_128.png"
	  },
	  "browser_action": {
	    "default_icon": "/img/icon_48.png",
	    "default_popup": "/html/popup.html"
	  },
	  "content_security_policy": "script-src 'self' https://ssl.google-analytics.com; object-src 'self'"
	}
	
Obviously I triple checked the path of the icon files, but I couldn't see anything wrong with them, so I Googled the error message, and I'd found this [Google Groups thread](https://groups.google.com/a/chromium.org/forum/?fromgroups=#!topic/chromium-extensions/4BSWseDPjZM). So it seemed that I wasn't the only one with this problem. It looked like that something had changed in Chrome and it no longer liked the paths of the icons. The strange thing was that I couldn't find anything about any relevant changes in the API [documentation](http://developer.chrome.com/extensions/manifest.html#icons) either.

I guess your first bet is to remove the trailing slash from the paths, and you're absoutely right! That was my first thing to do, but for some reason, when I updated the extension, it still didn't work. From that point I spent long hours of changing-zipping-uploading the extension but it kept throwing the error messages telling me that the manifest file was somewhat broken. After about 4 hours of trying I gave up… I thought it was probably Chrome being funny and I really had to wait until a new version comes out.

##The 'painful' aha moment

I just couldn't let it go that easy :) The next day I spent another couple of hours of investigating the issue and I found that whenever one updates an extension, the old version gets cached for quite some time. It was a pretty painful lighbulb moment…

So I went back to my very initial idea of removing that cheeky trailing slash from every single file path in the manifest file and when I updated the extension I took my time and let the server to take its time as well… After about 5 minutes I saw that the very recent version showed up in the [Details panel](https://chrome.google.com/webstore/detail/fontier/dkbamaalakfhckcidgiigdinhcncaeae/details) and when I hit the 'Add to Chrome' button, it worked!

I genuinely felt stupid for not taking the server caching into consideration, but at the same time I was super happy that eventually Fontier was back to action :)

So, just for your record, here is the new **manifest.json** file whithout any trailing slashes:

	{
	  "name": "Fontier",
	  "version" : "0.4",
	  "minimum_chrome_version": "22.0.1229.79",
	  "manifest_version": 2,
	  "description" : "The quickest way to change the default font size in Chrome. Great for testing Responsive websites using em and percentage units.",
	  "permissions": ["fontSettings"],
	  "icons": {
	    "16": "img/icon_16.png",
	    "48": "img/icon_48.png",
	    "128": "img/icon_128.png"
	  },
	  "browser_action": {
	    "default_icon": "img/icon_48.png",
	    "default_popup": "html/popup.html"
	  },
	  "content_security_policy": "script-src 'self' https://ssl.google-analytics.com; object-src 'self'"
	}
	

##Lesson learnt

So it turned out that it indeed was the trailing slash. I figure that something has changed in the extension packager and it no longer lets developers to use the trailing slash in file name paths - which is completely fine. The only problem whith that is that it is not documented and it turns out accidentally :) Luckily the problem wasn't a biggy, so it was pretty easy to sort.

I actually made the problem worse by not counting with the server caching and not checking the version numbers properly.

That tought me a damn good lesson of patience and to listen a bit more carefully to the tools that I use.
# CEP-FML (Still in progress)
Things to expect when creating your first CEP panel after developing scripts using ExtendScript, in a recommended order:

0.9: I recommend getting familiar with [Visual Studio Code](https://code.visualstudio.com). `If you are coming from a smipler text editor such as Atom this might feel like much. Take a few days to migrate, google / youtube search your struggles. In the long term, vsc is the better editor, even if a bit less user friendly for beginners.`

1. Install and learn some [node.js](https://nodejs.org/en/) basics, as well as [npm](https://www.youtube.com/watch?v=s70-Vsud9Vk). Get comfy with the console. `this will come in handy when you want to pack your extension for distribution, and later on for starting a new project using templates from github / your own template`

2. Lean [how to use git and github](https://www.youtube.com/watch?v=BCQHnlnPusY). `this will come handy for easier version control for your project, as well as using packages from github to kickstart a new project / improve your workflow`

3. Learn the basics of website development [HTML](https://www.youtube.com/watch?v=URSH0QpxKo8&list=PLRqwX-V7Uu6bI1SlcCRfLH79HZrFAtBvX), [JS](https://www.youtube.com/results?search_query=js+basics) and [CSS](https://www.youtube.com/watch?v=zGL8q8iQSQw&list=PLRqwX-V7Uu6bI1SlcCRfLH79HZrFAtBvX&index=7) `the extension panels are chromium tabs, you design your tool the same way you design a website, using HTML, CSS and JavaScript. This is different to creating a ScriptUI panel as you might be used to. A web page is written in HTML, styled using CSS and intereacts through JavaScript. If you can create a website, you can create a cep panel.`

4. Learn about [XML and JSON.](https://www.youtube.com/watch?v=rqROpUNb2aY). `JSON is a type of file/object you will see a lot. XML files are requires for extensions to run (read more about xml below)`

5. Learn about [manifest.xml](https://fenomas.com/2014/08/cep-5-getting-started-en/). `This is one of the required files to create an extension. This file is provided by Adobe in the `[CEP Resources](https://github.com/Adobe-CEP/CEP-Resources)` repository. You need to know that in the future this file might change based on Adobe CEP releases and changes. This file kickstarts your project, it includes information about the name of the bundle, how many panels does it have, which panels to show. It also points to the main index.html file that you will create for your extension. The host app (After-Effects in our case) then reads the manifest.xml file, and if everything makes sense to it and it is appy, it will go on to put your extension under window -> extensions -> myExtension. When you run your extension, it gets the index.html from the manifest file, and keeps going from there...`

6. Learn about [CSInterface.js](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/js/CSInterface.js) `That is another file adobe provides. This file is not essential for your extension to run, but it serves as a bridge between your javascript and your extendscript files using the 'evalScript()' function. In addition it extends on your JavaScript code in the context of your extension. Remember, javascript if originally for the web, it does not do stuff like "Get the skin color of After-Effects" or "Get details about the host app (After-Effects)". CSInterface.js, once inclued in your html in a <script></script> tag, lets you perform such tasks. Feel free to read the CSInterface.js file as it is pretty well documented, and may give you an idea about things you can do as part of your extension panel development journey.`

*intermission: At this point you own all the infinity stones. You understand that `manifest.js` and `CSInterface` are essentials for your extension, that you make your panel using `html, CSS and JavaScript`, and that managing your project makes much more sense using `nodejs, git and github`. You are now ready to create your first extension. Let's keep going!

7. Create your first extension project! Hurray! `I recommend that you create your extension in the following structure:`
```
myExtension/
        |
        |___src/
        |     |__ js/
        |     |      |_main.js
        |     |       |_CSInterface.js
        |     |
        |     |__ jsx/
        |     |       |_extendScript.jsx
        |     |
        |     |__ -html/
        |     |       |_index.html
        |     |
        |     |__ -css/
        |     |      |_style.css
        |     |
        |     |__ -assets/
        |     |
        |     |__ -CSXS/
        |            |_manifest.xml
        |
        |____dist/
        
```
`using a 'src' and dist' folders inside your extension folder gives you a space to place the final 'zxp' bundle for distributin. It also plays well with reload.js (read the next segment) as well as node.js and npm. In the future, we will include more files in the root extension folder in order to make the last steps of bundling your extension easier. Keeping your SOURCE CODE in the 'src' folder makes it easy to keep track of everything.

9. Learn about Good Boy Ninja's [reload.js](https://github.com/GoodBoyNinja/CEP-Auto-Folder-Copy). `This lets you work on your extension outside the After-Effects extensions directory. The benefit you get from this is that you can manage your project more easily. The extensions folder are sometimes read only and are usually not available for backup using stuff like dropbox. If you work in the extensions folder you are prone for using your files upon installation / uninstallation of extensions. Use reload.js to have nodemon copy your work directory to the extensions folder every time you are making changes to files.`

10. Learn about [playerDebugMode](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_8.x/Documentation/CEP%208.0%20HTML%20Extension%20Cookbook.md#debugging-unsigned-extensions) `In order to load an extension it needs to be signed (we will get to it). For development, you want to modify a file in your registry (windows) or using the terminal (mac) in order to allow After-Effects loading unsigned expressions like yours.`

*Hey! if the last step discouraged you are you were having trouble following it, you can enable/disable player debug mode [using this ZXP Installer](https://aescripts.com/knowledgebase/index/view/faq/zxp-installer-faq/). After installing, open the installer, go to settings and navigate to the last tab on the sidebar. Then, check "allow debugging" for at least the top 3 options.*

11. Edit your `manifest.xml` with the deatils about your extension. Use [Adam Plouff's manifest.xml](https://github.com/adamplouff/CEP-Skelotron/blob/master/src/CSXS/manifest.xml) as an example. Point the html to your index.html file.

11.1 With your basic html add a basic [html skeleton](https://www.w3schools.com/w3css/w3css_web_html.asp) in `index.html`.

12. Create a `.debug` file in your `src` folder [using this example](https://github.com/adamplouff/CEP-Skelotron/blob/master/src/.debug) and [download Chrome](https://www.google.com/chrome/). Edit your debug file so the Extension IDs match the Extension IDs you gave to your panels in the `manifest.xml`. `What is this file for? Keep reading...`

13. Relaunch After-Effects (your host app) and launch your extension from the Window -> Extensions menu. `if you don't see your extension, you either have not enabled playerDebugMode properly, your extension has not been copied properly to the extensions folder, or your manifest.xml content is incorrect. At this point it's time to google to make sure you've done the above the right way.

14. Congrats! You are ready to start working on your extension. If you think the past 14 steps were hell you'd be correct. All we can do is hope for adobe to make the proccess easier when they finalize [UXP](https://developer.adobe.com/photoshop/uxp/2022/) which will replace CEP. `don't worry, it gets worse`

15. Learn about Chrome DevTools. Remember that `.debug` file we created? Notice you list a port in there, for example: `Port="8073"`. If you've done everything correctly, **while your panel is open in After-Effects** you should be able to launch chrome, and go to `localhost:8073` (or whichever number you listed in that file for your **main panel**. If this works, you should be able to access DevTools, which lets you debug your extension using a console, an attribute inspector and other goodies. This intereacts directly with After-Effects.

16. In DevTools, disable mobile preview. It is on by default and it shows you a preview of your extension inside the browser. This makes the extension buggy, and the preview is very low quality. To disable it find the blue icon at the top which looks like this and click to disable: <img width="40" alt="Screenshot 2022-03-25 at 00 37 16" src="https://user-images.githubusercontent.com/66829812/160027039-df8e9d0a-1290-4d8e-90e6-5d91df55bf5e.png">

17. Ideally, you want to work with your extension open in After-Effects, and the DevTools right next to it. If you **ctrl+r** in the devtools window to refresh, your extension should refresh as well. Note: This will ***not*** reload any jsx files loaded through `manifest.xml`. If you want to see your jsx files changes taking place, you're going to have to reload your jsx files somewhere through your javascript files. An example for that would be in [Adam Plouff's extension skeletron](https://github.com/adamplouff/CEP-Skelotron/blob/master/src/js/app.js). Find the function called `loadJsxFile`. His function loads a very specific file. You might want to alter it a bit to make sure it finds your own jsx files. Then, call the function somewhere in your javascript. Now every time you refresh the extension, changes you made to your jsx file will be applied.

17.1 If you don't care about refreshing you can go old school. Changes you make to your code will update when you close your extension panel, and reopen it from Window -> Extensions -> My Extension. It's probably a good habit to do it from time to time when you feel like something isn't working correctly, almost like restarting your computer when it feels a bit grumpy and slow.

18. Design your panel using `html`, `css` and `javascript`. Use the DevTools to refresh the page quickly to see changes.`You could also use something called a 'live server' for this part in order to quickly see the changes in a browser WITHOUT using After-Effects. However, some functions from CSInterface and other stuff adobe provides will not work in the live server so you want to be aware of that.

19. Now you are gonna want to add extendscript functionality to your panel. JavaScript and ExtendScript are not one, they run as seperate engines, which brings you to...

20. Learn about [evalScript(), sync and async functions](https://medium.com/adobetech/using-es6-promises-to-write-async-evalscript-calls-in-photoshop-2ce40f93bd8b). `Use this tutorial to learn how to request after-effects to do stuff, and how to bring info back and forth between javascript and extendscript. Your main take away from this would be understanding that this proccess is asynchronous which makes it a pain in the ass.

21. After you tested your JS <-> ExtendScript skiils and you are feeling more comfortable about bridging the two, you can finally keep going with your creative flow and develop your extension, make changes to your panel, test functionalities, etc etc... `NOTE: If you are sturggling with this part, google evalScript() and you'll find a lot of wounded souls like yours begging for help.`






























------------------------------------------------------------------------------------------------
How to create an After-Effects extensions without losing your mind.
Actually, I am not convinced that this is possible, so at the very least I will try to loosley describe my experience, and things I wish somebody were to tell me along the way.

## Intro
I have just finished writing my first Extension from Adobe After-Effects. Saying that the proccess was rough is an understatement. I am writing this in hope to **define the path from start to finish** to hopefully help someone out there to create their own extensions.

## My experience
I started learning how to code because I wanted to learn expressions in After-Effects. Not long after I started learning scripting, extendscript, and finally extensions. My point of view is of a person who does not come from a web dev background.

# 1. What are extensions?
The extension panel is a chromium page in After-Effects.
Additionaly, they use Extenscript to communicate with After-Effects.
The two things are not one, JavaScript is not ExtendScript.
Because extensions are web pages, **you will want to eventually learn the basics of web development (at the very least html, css and JavaScript)**

# 2. Are you familiar with npm, github and nodejs?

If you are, you could clone [Adam Plouff's great repository](https://github.com/adamplouff/CEP-Skelotron) to help getting you started. If you are familiar with those technologies you might as well ditch this page and read his instructions for getting started.

Otherwise, I suggest that you start a bit smaller. Eventually after you feel a bit more comfortable with those technologies you should use Adam's skelotron project as a template, or if you are stubborn like me create your own template. Keep in mind that following this page would help you get **somewhere** and probably not at your destination. However, it might help you dimistfy some of the concepts. If you are new to this, there is SO MUCH to learn. I enjoy learning through failing.
Fuck around and take it slowly, if I can do this you can do this, seriously! Break through each step until you feel better about your knowledge.

Your goal is to eventually be able to point to all the cogs in the machine and explain what they are doing, why they are there and how you can coordinate between them. Once you feel like you can do that, revisit Adam's page and see how it's done right.
Eventually, you would probably want to create your own template at least once, just to make sure you fully understand the way the system works. Good Luck! You can do this!




# First steps
Create a folder with the name of your tool somewhere that it is comfortable for you to work from. I'll refer to this folder as `myExtension` on this page.

**Where?**
You might be tempted to create your extension folder where extensions are loaded from (in one of the following directories as mentioned in the [CEP Cookbook](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_8.x/Documentation/CEP%208.0%20HTML%20Extension%20Cookbook.md)):

```
Win(x86): C:\Program Files\Common Files\Adobe\CEP\extensions
Win(x64): C:\Program Files (x86)\Common Files\Adobe\CEP\extensions, and C:\Program Files\Common Files\Adobe\CEP\extensions (since CEP 6.1)
Mac: /Library/Application Support/Adobe/CEP/extensions
Per-user extension folder

Win: C:\Users\<USERNAME>\AppData\Roaming\Adobe\CEP/extensions
Mac: ~/Library/Application Support/Adobe/CEP/extensions
```

**Do not work from the extensions folder!**
you probably do not want to create your extension folder exactly there for multiple reasons:
- you might not have full access to save files from vsc
- it's harder to manage and backup your projects (especially if you are using something like dropbox)


~~Instead, create a synbolic link~~

**Instead, use my reload.js script**

It's much better that you use `nodejs` and `nodemon` using my `reload.js`: [reload.js](https://github.com/GoodBoyNinja/CEP-Auto-Folder-Copy)
Otherwise, you could use symbolic link (resources below) but it may puts your files at risk, as deleting extensions from a ZXP Installer can delete your original src directory as well. It's worth taking the couple of hours to learn the basics of `nodejs`.
If you prefer using a symbolic link, use the resources below:

More resources on symbolic link:

[for Windows with Powershell](https://www.youtube.com/watch?v=_VnONfOgP8M)

[for Mac with the terminal](https://www.youtube.com/watch?v=43mGItOoJIM)



# Folder Structure
You could use the [test extension](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_9.x/Samples/CEP_HTML_Test_Extension-9.0) from the CEP-Resources repository. However, I would suggest something more like:
```
myExtension/
        |
        |_src/
        |     |__ js/
        |     |__ jsx/
        |     |__ -html/
        |     |__ -css/
        |     |__ -assets/
        |     |__ -CSXS/
        |
        |_dist/
        
```
So far there are no files, only folders.
This folder structure is close to what you would find in [Adam's skeletron](https://github.com/adamplouff/CEP-Skelotron).
We would work inside the **src** folder and its sub folders. The dist folder will be a place to store the release versions.

# we need files
Get [this file](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/CSXS) called `manifest.xml`. If you are reading this in the future, you might want check if newer versions exists. sorry <3

Download `manifest.xml` and place it inside your CSXS Folder.

### The mess that is manifest.xml?
When after-Effects launches, it is looking for one and only file, `manifest.xml`.
Inside it there are details about the name of the extension, the version and other info.
It also points (read below) to two important places: `index.html` and `main.jsx`.

-Web pages are .html files. You are doing to want to create an `index.html` file in your `html` sub folder. The file does not have to be named "index", but it's a common convention.

-.jsx files are extendscript files in our case. If you wrote scripts before you know what they are. go ahead and create a `main.jsx` file in your jx sub folder. Same thing, the name doesn't matter.

### edit manifest.xml
the manifest file looks somewhat [like this.](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_9.x/Samples/CEP_HTML_Test_Extension-9.0/CSXS/manifest.xml).

Long and scary isn't it?
However, this is [Adam's manifest.xml file](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_9.x/Samples/CEP_HTML_Test_Extension-9.0/CSXS/manifest.xml)

It's a reallt good idea to compare the two. The manifest.xml from the Adobe repo is for a big test project the consits of multiple panels and windows. It's not exactly what you need, but you probably want to get comfortable with reading it and understanding how it works.


The key things you want to know about this file is that:
-You want to have a bundle name in `ExtensionBundleId`, will often look like `com.adobe.myExtensionName`.
-You want to define an <Extension> in the `<ExtensionList>` for each **panel** in your **extension** and name them accordingly. For example: `com.adobe.myExtensionName.Panel1`
-`ExtensionBundleVersion` is the version of your EXTENSION, as a package, as a tool.
-`HostList` is the list of apps that your extension supports. With ae: `<Host Name="AEFT" Version="13.0"/>`
- Inside `<DispatchInfoList>` you will find a pair of `<Extension></Extension>` tags **for each panel in your extension**. Their names correspond to the names you defined earlier in the `<ExtensionList>` and inside each one are a bunch of important and not so important stuff.
- On the more important stuff side: `<MainPath>` points to our `index.html` file. set it to `<MainPath>./html/index.html</MainPath>`. That way After-Effects will know that after reading this xml file, it will start reading the web page from `index.html`.
- After Effects also wants to knwo where the extendscript file is. That will be: `<ScriptPath>./jsx/core.jsx</ScriptPath>`

 
If this feels like a lot, it is because it is.
Take a few hours to write your own xml file, make changes, learn more about xml online and how data goes in tags.
This file is long and scary, **but at the end of the day it is the manifest, the kickstarter of your project. This file coordinates everything. This file has nothing to do with the way your extension looks, it just guides the host app (Ae) and tells it what the extension is, and where to start reading it from (and more).**
        
There is probably more formal information about the xml in the [CEP Cookbook](https://github.com/Adobe-CEP/CEP-Resources/blob/master/Documentation/README.md)
        


    
        
# Start creating your "web page"
Once you have an html project, you are good to go on working on the panel.
I am not going to teach you html, css and js on this page, but know that this is probably the next step for you, and it won't take a day. If you don't know anything about that, I would take a break and google html css and js tutorials, maybe make a basic web page or two.
        
In short, an `.html` file is the web page. It points to any additional `.css` files and `.js` files.
`.css` is a language used to design the web page and make it preeeeeety.
`.js` is JavaScript and helps you engae with the web page using scripting. That way you can get data, replace data, change data, add or remove data, etc. A lot of the intereactions of the user with buttons, sliders and other `html` elements will be created using `.js`.

        
Take your time with this and come back once you feel like you understand the relation between `html` `css` and `js` files.

        
# The files we have
By now we should have:
`Manifest.xml` to help Ae find out extension
`index.html` to build the panel, which is a web page.
`main.css` to design our panel nicely.
`main.js` to intereact with our web page.
`main.jsx` to intereact with After-Effects.
        
**We are missing one important file, [CSInsterface.js](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/js/CSInterface.js) **
        
# What is CSInterface.js?
It's a "library" of sort that adobe provides us. It gives us extra functionality over the extension using JavaScript.
Make sure to include it somewhere in your `src` folder (probably the `js` subfolder), and reference it in your html like so:
`<script src="../js/CSInterface.js"></script>`
        
### **CSInterface allows us to communicate with our jsx files!**
If included correctly in your html, through your javascript file you could write this comment:
```
var csInterface = new CSInterface();
csInterface.evalScript("alert('hello')");
```

That created an extendscript alert. If it works, you've done a good job so far.
You declared a CSInterface object. Once it's declared, you can keep using it and its `evalScript` method to execute stuff from your extendscript file.
        
### Learn about promises, async and await
Feels good when things are working right? Not yet.
evalScript is an async function and it returns a promise. This means that by the time it returns an answer from your `jsx` file your code keeps on running, and that might cause issues.

My advice is that you google some `await async javascript tutorial` and learn how this system works. Once you feel more comfortable with async functions, come back and try to create a working bridge between your javascript and extendscript engines.
        
        
 In my tool I ended up using a custom function, with the help of [this medium post](https://medium.com/adobetech/using-es6-promises-to-write-async-evalscript-calls-in-photoshop-2ce40f93bd8b).
        
```
  let customEvalScript = async function (script) {
        var result = await new Promise(function (resolve, reject) {
            GEP?.cs?.evalScript(script, resolve);
        });
        return result;
    };
        
```
        
Give the medium post above a read, it is very informative. I'll be honest, once I got comfortable with this system I kinda tried to forget about it as much as I can.
Try training yourself that everytime jsx is involved you have to use some sort of an async system to handle it.
        
        
        
      
# This page is still in progress. Byeeeee

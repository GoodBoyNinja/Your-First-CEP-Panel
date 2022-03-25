# CEP-FML (Or how to make your fiest CEP Panel)
Things to expect when creating your first CEP panel after developing scripts using ExtendScript, in a recommended order. 
*sorry about typos, I've been through enough pain to give a shit*

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

15. Learn about Chrome DevTools. Remember that `.debug` file we created? Notice you list a port in there, for example: `Port="8073"`. If you've done everything correctly, **while your panel is open in After-Effects** you should be able to launch chrome, and go to `localhost:8073` (or whichever number you listed in that file for your **main panel**.) If this works, you should be able to access DevTools, which lets you debug your extension using a console, an attribute inspector and other goodies. This intereacts directly with After-Effects.

16. In DevTools, disable mobile preview. It is on by default and it shows you a preview of your extension inside the browser. This makes the extension buggy, and the preview is very low quality. To disable it find the blue icon at the top which looks like this and click to disable: <img width="40" alt="Screenshot 2022-03-25 at 00 37 16" src="https://user-images.githubusercontent.com/66829812/160027039-df8e9d0a-1290-4d8e-90e6-5d91df55bf5e.png">

17. Ideally, you want to work with your extension open in After-Effects, and the DevTools right next to it. If you **ctrl+r** in the devtools window to refresh, your extension should refresh as well. Note: This will ***not*** reload any jsx files loaded through `manifest.xml`. If you want to see your jsx files changes taking place, you're going to have to reload your jsx files somewhere through your javascript files. An example for that would be in [Adam Plouff's extension skeletron](https://github.com/adamplouff/CEP-Skelotron/blob/master/src/js/app.js). Find the function called `loadJsxFile`. His function loads a very specific file. You might want to alter it a bit to make sure it finds your own jsx files. Then, call the function somewhere in your javascript. Now every time you refresh the extension, changes you made to your jsx file will be applied.

17.1 If you don't care about refreshing you can go old school. Changes you make to your code will update when you close your extension panel, and reopen it from Window -> Extensions -> My Extension. It's probably a good habit to do it from time to time when you feel like something isn't working correctly, almost like restarting your computer when it feels a bit grumpy and slow.

18. Design your panel using `html`, `css` and `javascript`. Use the DevTools to refresh the page quickly to see changes.`You could also use something called a 'live server' for this part in order to quickly see the changes in a browser WITHOUT using After-Effects. However, some functions from CSInterface and other stuff adobe provides will not work in the live server so you want to be aware of that.

19. Now you are gonna want to add extendscript functionality to your panel. JavaScript and ExtendScript are not one, they run as seperate engines, which brings you to...

20. Learn about [evalScript(), sync and async functions](https://medium.com/adobetech/using-es6-promises-to-write-async-evalscript-calls-in-photoshop-2ce40f93bd8b). `Use this tutorial to learn how to request after-effects to do stuff, and how to bring info back and forth between javascript and extendscript. Your main take away from this would be understanding that this proccess is asynchronous which makes it a pain in the ass.

21. After you tested your JS <-> ExtendScript skiils and you are feeling more comfortable about bridging the two, you can finally keep going with your creative flow and develop your extension, make changes to your panel, test functionalities, etc etc... `NOTE: If you are sturggling with this part, google evalScript() and you'll find a lot of wounded souls like yours begging for help.`

22. You have an extension. You created a nice panel and it does what it's supposed to do. You are ready to share it with the world.

23. WRONG! You are not ready. You will never. be. ready. It's time to test your extension on the other OS. You know, the one you are not using. Find a MAC or a PC and prepare your development setup all over again just to find out that some parts of your code are not working as expected. If this is the case for you, the most common reasons is the [differences in file paths](https://shapeshed.com/writing-cross-platform-node/) between the systems.

24. You fixed it, everything works now. Ready to share it with the world?

25. WRONG! You have not signed your extension! Rememeber `playerDebugMode`? Yeah, your users don't have this on. It's time to sign your extension. This must be as simple as signing your name somewhere or uploading your file to a server somewhere, right?

26. WRONG AGAIN! Signing your extension is not very straight forward. [Use this tutorial to sign your extension and create a ZXP file to share with the world.](https://www.youtube.com/watch?v=nN35aRJwX7g)

27. WRONG! -wait, but I didn't a.. -WRONG! I said WRONG! You are not ready. you will never be ready. `Before you are sharing your extension it's a good idea to test it out in different versions of After-Effects. The ` [CEP CookBook](https://www.youtube.com/watch?v=nN35aRJwX7g) ` mentions host app support for different CEP versions. For example, your extension might not work in an older version of After-Effects because the chromium verion it uses is older and supports different language features. If you are gonna share your extension online or in a marketplace, save yourself the pain of trying to figure out why only SOME users are having an issue with your extension, just to find out they are using an old version of After-Effects.

28. Share your ZXP with the world! You did it! It was painful, it wasn't worth it, but you did it. You are a better programmer. You are better at problem solving. Good Job.

# You did it! Now do it again:
You did a great job and you improved your skills a lot. It's easy to forget how not so long ago you used to make lame, boring panels using ExtendScript. Was it worth it? Only time will tell. However, if you made it to this point you are much more comfortable with `node.js`, `npm`, `html`, `JavaScript`, `ExtendScript`, `git` and `github`.

In order to not burn in the eternal hell of creating a new extension again and again, you might want to consider the following:
1. Use [clone Adam Plouff's Skeletron to start a new project](https://github.com/adamplouff/CEP-Skelotron#signing). `While I'm a stubborn piece of shit and I created my own template, Adam's Skeletron is a GREAT place to start a new project, and it streamlines the experience a LOT. The reason I did not refer you to his skeletron to begin with is that there is a lot to learn along the way. You need to be familiar with node, npm and other things you learned along the way in order to feel in control of something like Adam's skeletron. Assuming you are coming from ExtendScript and not from a webdev background, it was probably better to dive deep first. Now you are ready to make use of the kindness of others like you.`

2. Use [Adam's gulpfile.js](https://github.com/adamplouff/CEP-Skelotron/blob/master/gulpfile.js) to pack and sign your extension. `It requires little setup detailed in the skeletron repository, but it will also minify your JS, HTML and CSS files, as well as convert your ExtendScript files into binary (.JSXBIN) files, which helps protect your work and improves loading time. If you prefer going a different route, what I ended up doing is creating a somewhat similar setup using mostly nodejs and less gulp. nodejs is very powerful and gives you a lot of freedom when it comes to maniulating files. For example, I am using nodejs to also automatically add copyright headers to the top of my files as part of the packaging proccess. Ask yourself, what else you can do using nodejs when packaging your extension?`

3. Don't forget to go outside. Seriously. `It's really easy to lose yourself in the countless hours you had to spend learning all theses interesting yet furstrating technologies. Go outside, move your body. It's really good for you. Seriously.`

4. Find a community. CEP Developers is such a specific niche, everyone here experienced this very specific kind of pain. Find it on `twitter`, `discord` or `github`, say hi and share your pain (and your extensions :))

5. Branch out of CEP. `If you started at extendscript, you learned a whole lot of stuff by making your way here. Your new nodejs skills, html, css and javascript puts you in a position to learn even more cool stuff. You can now make web apps and cool web pages. Consider the possibilities! Also, there are a lot of GREAT technologies out there that do not involve you going through hell along the way. Not everything is like CEP, there's a lof ot neat stuff out there. Go have fun and explore other creatives ideas you've been keeping inside your big, juicy brain.`


That's was my expreicne. Now if you don't mind me, I have to go outside.
Good Boy Ninja


Other notable sources:
Refresh your panel: https://github.com/Adobe-CEP/Getting-Started-guides/issues/31


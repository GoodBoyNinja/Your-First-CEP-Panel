# CEP-FML
How to create an After-Effects extensions without losing your mind

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

Otherwise, I suggest that you start a bit smaller. Eventually after you feel a bit more comfortable with those technologies you should use Adam's skelotron project as it will give you a quick boost when you are starting and finalizing your projects. Until then, keep reading...







# First steps
Create a folder with the name of your tool somewhere that it is comfortable for you to work from. I'll refer to this folder as `myExtension` on this page.

**Where?**
You might be tempted to create your extension folder where extensions are loaded from (in one of the following directories as mentioned in the [CEP Cookbook](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_8.x/Documentation/CEP%208.0%20HTML%20Extension%20Cookbook.md)):

```
{
Win(x86): C:\Program Files\Common Files\Adobe\CEP\extensions
Win(x64): C:\Program Files (x86)\Common Files\Adobe\CEP\extensions, and C:\Program Files\Common Files\Adobe\CEP\extensions (since CEP 6.1)
Mac: /Library/Application Support/Adobe/CEP/extensions
Per-user extension folder

Win: C:\Users\<USERNAME>\AppData\Roaming\Adobe\CEP/extensions
Mac: ~/Library/Application Support/Adobe/CEP/extensions
}
```

**Do not work from the extensions folder!**
you probably do not want to create your extension folder exactly there for multiple reasons:
- you might not have full access to save files from vsc
- it's harder to manage and backup your projects (especially if you are using something like dropbox)

_You might ask yourself "Am I supposed to copy my extension folder to the extensions folder each time I want to preview the change in After-Effects?"._

**Instead, create a synbolic link**
So as [Adam Plouff recommends](https://github.com/adamplouff/CEP-Skelotron#usage) you can create a symbolic link.

In short, a synbolic link is like a shortcut to your folder, but unlike a shortcut it behaves like the same folder. This means that if your extension folder `C:/Users/${userName}/desktop/myExtension/` has a link to in in 
`C:\Users\<USERNAME>\AppData\Roaming\Adobe\CEP\extensions\myExtension\`, After-Effects will actually be able to read your those files and recognize your extension.

*If you are on pc, I have found that it's much easier to create a symbolic link through powerShell (instead of cmd). 

a symbolic link using powershell would look something like this:

`New-Item -ItemType SymbolicLink -Path "C:\Users\<USERNAME>\AppData\Roaming\Adobe\CEP\extensions\myExtension\" -Target "C:/Users/${userName}/desktop/myExtension/"`

using the terminal on a mac, it should look somewhat like this:

`ln -s "C:/Users/${userName}/desktop/myExtension/" C:\Users\<USERNAME>\AppData\Roaming\Adobe\CEP\extensions\myExtension\`

More resources on symbolic link:
[for Windows with Powershell](https://www.youtube.com/watch?v=_VnONfOgP8M)
[for Mac with the terminal](https://www.youtube.com/watch?v=43mGItOoJIM)


```diff
- An important note about Symbolic Link:
- With symbolic link, deleting files or even the folder itself from the "linked" instance (the one in the extensions folder) affects the original folder.
-It's really important that you understand that and make backups to your project often, or use git and github.
-That way you can make sure there is an online copy of your files from time to time.
-Dropbox saved my butt once with their time machine, so keep that in mind.
```


# Folder Structure
You could use the [test extension](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_9.x/Samples/CEP_HTML_Test_Extension-9.0) from the CEP-Resources repository. However, I would suggest something more like:
```
{
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
        }
```
So far there are no files, only folders.
This folder structure is close to what you would find in [Adam's skeletron](https://github.com/adamplouff/CEP-Skelotron).
We would work inside the **src** folder and its sub folders. The dist folder will be a place to store the release versions.

#we need files
Get [this file](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_9.x/Samples/CEP_HTML_Test_Extension-9.0/CSXS) called `manifest.xml`. If you are reading this in the future, you might want check if newer versions exists. sorry <3

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



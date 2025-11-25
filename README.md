> [A year ago](./deprecated/original.md) I wrote a long detailed post in an attempt to help ExtendScript developers to jump ship and start developing CEP panels.
While it accurately describes my experience building my first CEP panel, the advice given is wrong, dated and misleading.
Please read my updated version:


### **First, admit that you are NOT ready**     

Admittedly, online information about CEP development is lacking. If you are coming from ExtendScript background (meaning, you basically just learned how to program and created a couple of script panels for After-Effects), I recommend you to keep in mind this:
>       🧠 All CEP resources online, even Adobe's cookbook-so-called-"getting started"-guide,
>       are written for web developers.
>       Since you are not a web developer (yet), trying to follow even the most
>       basic guides will be a discouraging and long struggle
>       that will result in you giving up.

You are currently at a point where you are used to making scripts, which **just work**.
CEP development is the opposite. At first, **nothing works**, and you have to slowly build things together piece by piece to make things work. Incredible projects such as [Bolt-CEP](https://github.com/hyperbrew/bolt-cep) help making the experience more pleasant, but you as an ExtendScript developer will still have to learn a lot of new things just to get things to work.

### **So, where do I start?**
>        📆 Clear up the next two weeks of your schedule. We are becoming web developers!

My advice to you is to expect building your first CEP panel in a couple of weeks from today. For the next two weeks you are going to learn the basics of modern web development instead by building a static website from start to finish. 

The topics you want to cover are:
- Visual Studio Code
- HTML
- CSS
- Modern JavaScript
- - [ES6 Methods](https://www.w3schools.com/js/js_es6.asp)
- - [Async/Await and Promises](https://www.youtube.com/watch?v=QO4NXhWo_NM&list=PLRqwX-V7Uu6bKLPQvPRNNE65kBL62mVfx&index=1&t=1329s)
- - ES6 Modules (import/export) `Learn how to work with multiple files`
- - Package Managers (npm, yarn, etc) `Learn how to install and use packages from the internet`
- - [Module Bundlers & Live Server](https://www.youtube.com/watch?v=5IG4UmULyoA) (You're gonna probably use [vite](https://vite.dev/)) `Learn how to take all your files and bundle them into one file`
- [Git & Github](https://www.youtube.com/watch?v=BCQHnlnPusY) `Version control for your project, as well as a place to store your code online`
- A little bit of [XML](https://www.w3schools.com/xml/) (it's very similar to HTML)
- A little bit of [JSON](https://www.w3schools.com/js/js_json_intro.asp) (it's very similar to JavaScript objects)
- Some basic [Node.js](https://nodejs.org/en/) terminal commands (Making a folder, moving files, etc)

### **Why are there so many things to learn?**
[This Video](https://youtu.be/QliwSwWHJoQ) does a great job at explaining the complexity of modern web development.

### **I learned it all, what now?**
Now that you have the skills, you can choose between the easy way and the hard way.

#### **The easy way**
Use one of the following:
*  [⚡ Bolt-CEP](https://github.com/hyperbrew/bolt-cep) - A great tool that will help you get started with CEP development. This is probably the most production ready starting point you can find. The downside is that it relies on frameworks such as React, Vue and Svelte. It's totally doable to learn one of these (Probably Svelte would be the fastest to learn), but it's still some extra work.
* My [🌼 CEP-Vite-Template](https://github.com/GoodBoyNinja/cep-vite-template). This is what I use for my projects, it's a simpler lo-fi solution that also uses Vite but without any frameworks. You might find it easier to start with, but it's not as production ready as Bolt-CEP.



#### **The hard way**
So you made it here...
The hard way (or as I like to call it: *my way because I was a stubborn idiot*) is to set up everything from scratch. It is not worth it if you are just dying to start building your first CEP panel. However, if you are curious and want to learn how everything works, this is a good way to do it.

- Symlink ([MacOS](https://www.howtogeek.com/297721/how-to-create-and-use-symbolic-links-aka-symlinks-on-a-mac/) / [Windows](https://www.howtogeek.com/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/) / [Node.JS](https://www.geeksforgeeks.org/node-js-fs-symlink-function/)) `Place your extension file anywhere on your machine, but have a symlink to it in the CEP extensions folder, which will live update to your changes`
- [./CSXS/manifest.xml](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/CSXS) `This is the file that tells CEP where to find your extension, what it's called, what it's icon is, etc. If this file is not set up correctly, your extension will show up at all in the extensions list in the app you are developing for. It must be placed inside a folder names CSXS, which must be placed  in the root of your extension folder.`
- [CSInterface.js](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/js/CSInterface.js) `A module Adobe provides to help you run ExtendScript from your CEP panel, as well as other useful functions`
- [evalScript()](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Samples/CEP_HTML_Test_Extension-10.0/js/CSInterface.js#L529) `A function provided by CSInterface.js that allows you to run ExtendScript from your CEP panel. This function is asynchronous, meaning it will not wait for the ExtendScript to finish running before continuing.`
- [CEP versions](https://github.com/Adobe-CEP/CEP-Resources/blob/master/Documentation/README.md) `Both manifest.xml and CSInterface.js are provided by Adobe. However, different versions exist online. Usually you want to work with the latest versions, but those may not be compatible with older versions of the app you are developing for.`
- [Namespacing your JSX Files](https://hyperbrew.co/blog/top-2-extendscript-mistakes-and-how-to-avoid-them/) - `To avoid clashes with other extensions and scripts, you should namespace the content of your JSX files. This is not mandatory, but it will help you keep your sanity later on.`
- [Signing your extension](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Documentation/CEP%2011.1%20HTML%20Extension%20Cookbook.md#signing-extensions) - `This is the last step before publishing your extension and is mandatory. You want to integrate it with your build process, so that it happens automatically.`
- [Debugging](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Documentation/CEP%2011.1%20HTML%20Extension%20Cookbook.md#debugging-unsigned-extensions) - `Adobe's way to let you use Chrome DevTools while your extension is running.`

In general, after you acquired the knowledge of web development it will not be too hard to follow online guides, including the [Adobe CEP CookBook](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_11.x/Documentation/CEP%2011.1%20HTML%20Extension%20Cookbook.md)

>       💡 Important note: Don't go the hard way. Just use Bolt-CEP.



# **A word of encouragement**
There is a lot to learn before you can start creating sleek and beautiful CEP panels. However, it is not as hard as it seems.

If anything, modern web development is easier than it has ever been. The tooling, resources and documentation available for web development is so good, it will make you hate ExtendScript even more.

If [I did it](https://www.goodboy.ninja/skew) (and I am not a web developer), you can do it too. Take as much time as you need to learn the basics of web development. It will be worth it in the end.






---
---

🤖 If you still wish to [read my unsorted thoughts from last year](./deprecated/original.md), feel free!

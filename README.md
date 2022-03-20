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
Extensions are just a folder. Create a folder with the name of your tool somewhere that it is comfortable for you to work from. I'll refer to this folder as `myExtension` on this page.

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











#What's in my folder?




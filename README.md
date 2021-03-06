# DConsole

####Donsole-online
The console is available online at [haxedomain.com](http://www.haxedomain.com/console).
___________
**DConsole** or **The Console** is a real-time console that allows you to:
 
* Run scripts.
* Access and modify fields and objects.
* Call registered functions.
* Monitor fields.
* Customize appearence.
* Profile the app in realtime.
* Register new commands that respond to user input.


**Latest Changes - 4.3.1**
* Added Html5/JS basic support, see notes at the end of this README
* Updated to hscript 2.0.4 (older versions may experience bad behaviour)
* Small fixes

For more changes or other versions, see [CHANGELOG](https://github.com/ProG4mr/dconsole/blob/master/CHANGELOG). <br/><br/>
The console supports **flash**, **neko** and **cpp** targets using **openfl**, **html5** is also supported using [jquery-terminal](http://terminal.jcubic.pl/) and does not require openfl (see instructions below), other input/output implementations may also be added.


### Install

```
haxelib install dconsole
```
____________

### Getting Started

Using dconsole is straightforward:

```actionscript
import pgr.dconsole.DC;

DC.init();
DC.log("This text will be logged.");
DC.registerFunction(this.testFunction, "myfunction");
DC.registerObject(this, "myobject");
DC.registerClass(Math, "Math");
```

To show the console, press **TAB**, then type **"help"** or **"commands"**
to see what commands are available, also **"objects"** and **"functions"** are useful commands to show registered objects and functions.

![help](http://i1148.photobucket.com/albums/o562/ProG4mr/dconsole1_zps2287758b.png "help")

The console can be used to control your application, accessing registered objects and functions in realtime
without having to re-compile. Its also possible to evaluate complex expressions and scripts using haxe sintax.

![example](http://i1148.photobucket.com/albums/o562/ProG4mr/dconsole2_zpsa362d475.png "example")

_____________
### Monitor<br />

The monitor allows you to register fields and monitor their value in real time.
For example, to monitor a display object x position:
```js
DG.monitorField(player, "x", "playerX");  
```

Pressing **CTRL + TAB** brings up the monitor that shows the variable updated in real time
![monitor](http://i1148.photobucket.com/albums/o562/ProG4mr/monitor_zps1cba1388.jpg "monitor")
<br />The screenshot shows monitor being used in Adam Atomic's Mode demo.
____________
### Profiler<br />

The profiler is lightweight and portable tool that shows: 

* What code is eating more cpu.
* How many times is some code executed inside other code block.
* How much time code takes to execute (benchmark)
* Other statistics not shown by default like absolute elapsed, min, max, totalInstances etc..

To sample a code block do:
```js
DC.beginProfile("SampleName");
// Code goes here
DC.endProfile("SampleName");
```
Toggling the profiler with **SHIFT + TAB** shows real-time statistics that are updated according to refresh rate. <br />

![profiler](http://i1148.photobucket.com/albums/o562/ProG4mr/profiler_zps30be5bb6.jpg "profiler")
<br />The screenshot shows the profiler using multiple nested samples, idents are used to vizualize the samples hierarchy.<br />

* **EL** elapsed milliseconds
* **AVG** average elapsed milliseconds
* **EL(%)** elapsed percentage
* **AVG(%)** average elapsed percentage
* **#** Occurrences of sample inside root sample
* **Name** Sample name
<br />

_________________
### HTML5 / JS

DConsole can run on html5 / javascript using jquery-terminal to process input and log console output.
To do so, first download [jquery-terminal](http://terminal.jcubic.pl/) and place **jquery.terminal-x.x.x.min.js** and **jquery.terminal.css** inside your export directory. 

Then open your ```index.html``` (flashdevelop generates this automaticaly with haxe-js template)  and add the following:

1. Add ```<link rel="stylesheet" href="jquery.terminal.css"/>``` inside ```<head></head>```
2. Add the tag ```<div id="console"/>``` inside ```<body></body>``` 
4. Add the following lines before your generated JS is executed (in flashdevelop template: ```<script src="project_name.js">```):
 
   ```html
   <script src="http://code.jquery.com/jquery-2.1.1.min.js" ></script>
	<script src="jquery.terminal-0.8.8.min.js"></script>
	<script>
        terminal = $('#console').terminal(function(command, term) {
            if (command !== '') {
                pgr.dconsole.DC.eval(command);
            } else {
                term.echo('');
            }
        }, {
            greetings: false,
            name: 'dconsole',
            width: "100%",
            height: "200",
            prompt: '> ',
        });
        
        document.addEventListener('console_log', function(evt) {
            terminal.echo(evt.detail.data, {
                finalize: function(div) {
                    div.css("color", "#" + evt.detail.color);
                }
            });
        });
	</script>
   ```

What this does is loading jquery from web, load jquery-terminal locally, and assign a jquery terminal with proper configurations to the div with id:console created earlier.


![consolehtml](http://i1148.photobucket.com/albums/o562/ProG4mr/consolehtml5_zpsb26d8bc6.png)

Not the most elegant way to get it working but it surely works well.<br/>
Profiler and monitor support will be added soon aswell as a webpage to experiment with haxe and dconsole online.<br/>
**Important** : make sure dead code elimination is off with ```-dce no``` flag, otherwise problems do occur.

________________
### Tips<br />

* Console can have full height using **DC.init(100)** where 100 means 100% parent sprite height.
* To visualize more info when errors occur use **DC.setVerboseErrors(true)**.
* Registering commands using **DC.registerCommand()** allows to add custom behavior based on input.
* Registering classes using **DC.registerClass()** allows to access static functions and methods of that class.
* Private functions, private fields and getters/setters can also be used.
* **DC.eval()** can be used to evaluate code directly.

__________________
### Suggestions / Comments / Bugs 

[Email me](mailto:prog4mr@gmail.com) suggestions, comments, bug reports etc..<br />
Or create a new issue (even better). 

### Contributions

Bug reports and feedback are welcomed.

To contribute code make sure tests work by running tests/testFlash.bat, testNeko.bat and testWindows.bat. Update tests or write new ones if necessary, the goal is to make sure the console basic functionality 
is working on all targets.

**Thanks** to every one supporting to this project so far!


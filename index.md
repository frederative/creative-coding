# welcome

hi there!

# completed projects

logarithmic:

![logarithmic](https://assets.objkt.com/file/assets-001/KT1RJ6PbjHpwc3M5rw5s2Nbmefwbuwbdxton/5/6/467356/artifact.png)

the intent of this piece was to center particles around a logarithm and let them find their way.  at various points along the curve i let 2 particles loose, one going up (colorful) and one going down (black & white).  each particle had some jank added to their movement to make it look interesting.

# helper classes

* Recording: [CCapture.js](https://github.com/spite/ccapture.js)

This one took me a while to get setup, and for some reason it **only** works for me in Firefox (Chrome chokes on it).  Here are the relevant lines of code I insert to make this work (note: most of this is defined in the CCapture page as well).

**index.html**:

```
<script type="text/javascript" src="CCapture.all.min.js"></script>
```

**script.js**

```
// at the top:
let fps = 30;
var capturer = new CCapture({ format: 'png', framerate: fps });
...
function setup() {
  ...
  frameRate(fps);
}

function draw() {
  if (frameCount === 1) {
    capturer.start();
  }
  ...  
  if (done) {
    console.log("done");
    noLoop();
    capturer.stop();
    capturer.save();
  }
  capturer.capture(document.getElementById('defaultCanvas0'));
}
```

I'll run a local webserver with Python (there are *numerous* ways to do this, I use Python regularly and am lazy in installing other methods ... this also works).

`$ python3 -m http.server`

This creates a simple webserver on port 8000, so you can easily pop open Firefox and visit 127.0.0.1:8000 (or localhost:8000, if you're not into the whole verbosity thing).  The sketch will run once and bail when it hits the end case and then generate a `tar` archive of your PNGs.  Extract it somewhere you know and love and then you'll have a directory full of PNG images.

I then stick the PNGs together with [gifski](https://gif.ski/), a delightful terminal application for creating high-quality gifs. Usually, my terminal resembles the following:

```
> Be inside the folder of recorded images

$ gifski --output mygif.gif --fps 30 --quality 100 *.png
```

Sometimes I toss on an ` && rm *.png` as well, but be warned that this command deletes all the images in that directory.

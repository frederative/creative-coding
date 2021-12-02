# welcome

hi there!

# completed projects

logarithmic:

![logarithmic](https://assets.objkt.com/file/assets-001/KT1RJ6PbjHpwc3M5rw5s2Nbmefwbuwbdxton/5/6/467356/artifact.png)

the intent of this piece was to center particles around a logarithm and let them find their way.  at various points along the curve i let 2 particles loose, one going up (colorful) and one going down (black & white).  each particle had some jank added to their movement to make it look interesting.

# helper classes

## Recording: [CCapture.js](https://github.com/spite/ccapture.js)

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

## Particles

This is just a homebrew little Particle class that I use to give each particle the ability to do its own thing. I am 100% certain there are better (i.e., performant) ways to do this, but this is quick and dirty and gets the job done.

```
let particles;
class Particle {
  constructor(x, y, vx, vy, col, size) {
    this.position = createVector(x, y);
    this.velocity = createVector(vx, vy);
    this.color = col;
    this.size = size;
  }
  
  // return true if in-bounds, false otherwise
  update() {
    this.position.x += this.velocity.x;
    this.position.y += this.velocity.y;
    if (this.position.x < 0 || this.position.x > width || this.position.y < 0 || this.position.y > heigh)
      return false;
    return true;
  }
  
  // circle-friends
  draw() {
    noStroke();
    fill(this.color);
    circle(this.x, this.y, this.size);
  }
}

function setup() {
  let num_particles = 42;
  particles = [];
  // add particles
  for (let i = 0; i < num_particles; i++) {
    particles.push(new Particle(
      random(width), random(height); // position
      random(-5, 5), random(-5, 5),  // velocity
      color(random(255)),            // grayscale color
      random(1, 5)                   // size
    ));
  }
}

function draw() {
  ...
  
  // update, draw, and snip dead particles
  for (let i = particles.length-1; i >= 0; i--) {
    let r = particles[i].update();
    if (r) 
      particles[i].draw();
    else
      particles.slice(i, 1);
  }
  if (particles.length === 0) {
    console.log("done");
    noLoop();
  }
}
```

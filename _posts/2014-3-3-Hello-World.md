---
layout: post
title: Creating a 3D Magnetic Compass in P5JS
---

### Background

I have been dabbling in 3D modelling with [Blender](https://blender.org) with a view to bring some historical instruments and experiments to life. I was very excited when I discovered that there are javascript libraries such as [ThreeJS](threejs.org) that can directly import entire scenes from Blender. Threejs seemed a bit complicated to start with when I'm still very new to javascript itself. So I decided to take my first little dips with one of the friendliest javascript libraries - [P5JS](https://p5js.org).

![_Ampere's astatic needle_]({{ site.baseurl }}/images/2021-06-27-ampere-astatic.png)

The first 3D historical instrument project I have in mind is Ampere's 'astatic needle'. When Oersted discovered electromagnetism, he never actually observed a magnetic needle becoming perpendicular to a current-carrying wire. The action of the Earth's magnetic field made it a smaller angle of deflection. Ampere invented an elaborate apparatus where he could rotate a magnetic compass such that the needle's axis was parallel to the Earth's magnetic field vector at a place. This meant that the magnetic effect of an electric current on the needle could be studied in isolation. Using this apparatus which he called an 'astatic needle', he showed that the deflection was 90 degrees every single time, even with small currents.

### First step

Since implementing a simulation of the astatic needle involves complicated vector calculations, I decided to start in a small way by first creating a simple magnetic compass kept in a horizontal position. In the first version, I ignored even the dip of the magnetic field and assumed it to act horizonatlly towards the geographical north.

I made a basic model of a magnetic compass in Blender. It had two parts - a dial and a needle. Both parts were independently exported as .obj files. Obj is not the best format since it doesn't transfer material properties and relationships between objects, but that's what P5JS supports. When I transition to ThreeJS later, I see myself using the more versatile GLTF format.

![_Blender model_]({{ site.baseurl }}/images/2021-06-27-blender-screenshot.png)


### Writing the first P5JS script

First, I created an HTML file. The links to the P5JS library as well as the local script for the magnetic compass are included in the head.

    <html>
      <head>
        <script src="https://cdn.jsdelivr.net/npm/p5@1.3.1/lib/p5.js"></script>
          <script src="sketch_v1.js"></script>
      </head>
      <body>
        <main>
        </main>
      </body>
    </html>

P5JS script for the compass:

    let compass;
    //let needleRotY=0;
    let p1, p2, p3, p4, p5, p6, p7;
    const B = 40e-6 //magnetic field of the Earth
    const u = 60 //magnetic moment in Am^2
    const I = 80e-7 //moment of inertia of the needle
    let tau, alpha;
    const res = 50e-6;
    let omega = 0;
    let time = 0;
    let theta = 0;

    function preload() {
      compassNeedle = loadModel('compass-needle.obj');
      compassDial = loadModel('compass-dial.obj');
    }

    function setup() {
      createCanvas(400, 400, WEBGL);
      p1 = createP('theta = '+ theta*180/PI);
      p2 = createP('B = ' + B + ' T');
      p3 = createP('u = ' + u + ' Am<sup>2</sup>');
      p4 = createP('I = ' + I + ' kgm<sup>2</sup>');
  
      tau = u * B * sin(theta);
  
      if (omega > 0) {
        alpha = (tau + res) / I;
      } else if (omega < 0) {
        alpha = (tau - res) / I;
      } else {
        alpha = tau / I;
      }
  
      p5 = createP('alpha = ' + alpha + ' rad/s<sup>2</sup>');

      p7 = createP('omega = ' + omega + ' rad/s');
      p6 = createP('time = ' + time);

      camera(0, -300, 200, 0, 0, 0);
    }

    function draw() {
      background(200);
  
      orbitControl();
  
      push();
      //rotateX(frameCount * 0.01);
      //rotateY(frameCount * 0.01);
      rotateY(theta);
      scale(100);
      model(compassNeedle);
      pop();

      tau = u * B * sin(theta);
  
      if (omega > 0) {
        alpha = (tau + res) / I;
      } else if (omega < 0) {
        alpha = (tau - res) / I;
      } else {
        alpha = tau / I;
        console.log('calculating')
      }

      //  alpha = tau / I;

      p5.html('alpha = ' + alpha + ' rad/s<sup>2</sup>');

      omega -= alpha * deltaTime/1000;
      p7.html('omega = ' + omega + ' rad/s');
  
      theta += omega * deltaTime/1000;
      p1.html('theta = ' + theta*180/PI);
    
      time += deltaTime;
      p6.html('time= ' + time/1000);
  
      push();
      //rotateX(90);
      //rotateY(0);
      scale(100);
      model(compassDial);
      pop();
  
      if (abs(theta) < 0.1 && abs(omega) < 0.1) {
        theta = 0;
        omega = 0;
      }
    }

    function keyTyped() {
      if (key === 'r') {
        theta -= 0.5;
      } else if (key === 'l') {
        theta += 0.5;
      }
      return false;
    }



The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

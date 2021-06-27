---
layout: post
title: Creating a 3D Magnetic Compass in P5JS - Part I
---

### Background

I have been dabbling in 3D modelling with [Blender](https://blender.org) with a view to bring some historical instruments and experiments to life. I was very excited when I discovered that there are javascript libraries such as [ThreeJS](threejs.org) that can directly import entire scenes from Blender. Threejs seemed a bit complicated to start with when I'm still very new to javascript itself. So I decided to take my first little dips with a javascript library that couldn't be any friendlier - [P5JS](https://p5js.org).

My first 3D historical instrument project in mind is Ampere's 'astatic needle'. When Oersted discovered electromagnetism, he never actually observed a magnetic needle becoming perpendicular to a current-carrying wire. The action of the Earth's magnetic field made the angle of deflection smaller. Ampere invented an elaborate apparatus where he could rotate a magnetic compass such that the needle's axis was parallel to the Earth's magnetic field vector at a place. This meant that the magnetic effect of an electric current on the needle could be studied in isolation. He showed that the deflection was 90 degrees every single time, even with small currents.

### First step

Since implementing a simulation of the astatic needle involves complicated vector calculations, I decided to start in a small way by first creating a simple magnetic compass kept in a horizontal position. In the first version, I ignored even the dip of the magnetic field assumed it to act horizonatlly towards the geographical north.

![_config.yml]({{ site.baseurl }}/images/404.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.

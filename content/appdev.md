+++
title = "Application development"
weight = 3
date = "2019-03-12"
+++

# Upgrading legacy code
I have recently upgraded two applications to high performant rust code. Because of the design of the rust language, many applications are faster "out of the box" after implementation in rust. The two test cases that I have worked with are:

1. Rapidly rotating Neutron Star (RNS): C code was written in 1995 by <a href="
http://www.astro.auth.gr/~niksterg/" target="_blank">Nikolaos
Stergioulas</a> and updated in 1999 by <a href="
https://apps.ualberta.ca/directory/person/morsink" target="_blank">Sharon Morsink</a>. The code calculates the rotation of neutron stars in general relativity. The original website for the C code is <a href="http://www.gravity.phys.uwm.edu/rns/" target="_blank">UWM Center for Gravitation, Cosmology & Astrophysics - Rapidly Rotating Neutron Star</a> and I have a Github repository of it <a href="https://github.com/pauleaster/rns.v2.0" target="_blank">here</a>. After converting this code I found that I achieved a ***2x*** improvement in speed compared to the original C code.

2. Python medical software designed to read binary data for a CPAP machine and convert that data to csv format. I found that I achieved a ***20x*** speed improvement for reading the binary data from disk and saving the resulting csv to disk.

The two case studies will be outlined below:

# RNS Code case study: C to rust
<video width="320" height="240" controls>
    <!-- <source src="/rns_c_version.ogg" type="video/ogg" playsinline> -->
    <!-- <source src="/rns_c_version.webm" type="video/webm" playsinline> -->
    <source src="/rns_c_version.mov" type="video/mov" playsinline>
    <source src="/rns_c_version.mp4" type="video/mp4" playsinline> 
Your browser does not support mov video tags.
</video>
<!-- <video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
Your browser does not support the video tag. -->
</video>

# CPAP medical data case study: python to rust


+++
title = "Application development"
weight = 20
date = "2021-12-14"
+++

# Upgrading legacy code
I have recently upgraded two applications to high performant <a href="https://www.rust-lang.org" target="_blank">rust</a> code. Because of the design of the rust language, many applications are faster "out of the box" after implementation in rust. The two test cases that I have worked with are:

1. Rapidly rotating Neutron Star (RNS): C code was written in 1995 by <a href="
https://www.astro.auth.gr/~niksterg/" target="_blank">Nikolaos
Stergioulas</a> and updated in 1999 by <a href="
https://apps.ualberta.ca/directory/person/morsink" target="_blank">Sharon Morsink</a>. The code calculates the rotation of neutron stars in general relativity. The original website for the C code is <a href="http://www.gravity.phys.uwm.edu/rns/" target="_blank">UWM Center for Gravitation, Cosmology & Astrophysics - Rapidly Rotating Neutron Star</a> and I have a Github repository of it <a href="https://github.com/pauleaster/rns.v2.0" target="_blank">here</a>. After converting this code I found that I achieved a ***2x*** improvement in speed compared to the original C code.

2. Python medical software designed to read binary data from a CPAP machine and convert that data to csv format. I found that I achieved a ***20x*** speed improvement for reading the binary data from disk and saving the resulting csv to disk.

The two case studies will be outlined below:

# RNS Code case study: C to rust
The Rapidly rotating Neutron Star code is approximately 3000 lines of C code. I manually converted each line of C code to rust code ensuring that the resulting calculations achieved the same numerical result. See my Github repository for my copy of the  <a href="https://github.com/pauleaster/rns.v2.0" target="_blank">RNS C code</a> and the <a href="https://github.com/pauleaster/rns.rs" target="_blank">RNS rust code</a>.

Some motivations for updating this code-base are: 

1.  Rust is essentially platform agnostic and can produce binaries that run on *nix, Mac, and Windows. 

2.  Testing is built into the language. Although rust does not use an interpreter, it is a simple matter to add unit tests to a function. This enables testing the functions as they are written.

3.  Rust has an extensive library infrastructure though not as extensive as python. The libraries in rust are called crates, and it is a simple matter of adding them to the `config.toml` file and adding in a `uses` statement to allow access to the libraries functions and data structures.

4. Rust is fast! In a case-by-case situation, it may not be faster than original C or C++ code. But it is in the same ballpark in speed comparison.

5.  A very important benefit resulting from converting C and C++ to rust is memory safety. Rust enforces memory safety at compile time and significantly reduces the chance of memory bugs (for example segment faults, a.k.a use after free). As this is performed at compile time there is no impact on performance. Furthermore, rust does not have a garbage collector with its associated performance hit.

6.  Finally, rust is strongly typed which helps eliminate many errors.

The RNS code was converted function by function, line by line, and variable by variable. No effort was taken to analyse the code and refactor to increase performance. The algorithms in this code are complex, they describe the space-time curvature and matter/energy density of rotating neutron stars according to Einstein's general relativity and stellar astrophysics. 

The primary goal was to get a fully working codebase in rust in the shortest possible timeframe. Additionally, I kept the number of crates (libraries) as small as possible and used standard rust as much as possible. The resulting code used the `nd_array` crate for two-dimensional matrices and three-dimensional tensors and the `csv` crate for reading in the equation of state file. Two other crates were used for testing float values. Additional code was added to allow contour plots of the resulting data and used the `plotly` crate. Rust datatypes were chosen to either be as close as possible to the C variable, or selected to make implementation easier. Variable names were kept the same, except for case changes to follow the rust naming conventions. After completing the conversion, I ran `flamegraph`, a profiling tool, to see where the code was using the most CPU resources and implemented a simple change to speed up the rust program even more.

After fixing a few minor bugs, mostly associated with off-by-one errors in indices, the rust code ran almost immediately -  achieving a less than `1e-11` fractional error in values when compared to the original C code! The amount of debugging was minimal, as each function was tested as it was written. What was surprising and not expected, was that the rust code ran about twice as fast as the C code! I haven't explored why this is the case, but it was a very interesting result. 

The following two videos show the execution of the C code compared to the rust code:

<div class="column">
    <video width="320" height="240" controls>
        <source src="/rns_c_version.mov" type="video/mov" playsinline>
        <source src="/rns_c_version.mp4" type="video/mp4" playsinline> 
    Your browser does not support mov video tags.
    </video>
    <p>This is the original C code, execution time=2.35s</p>
</div>
<div class="column">
    <video width="320" height="240" controls>
        <source src="/rns_rust_version.mov" type="video/mov" playsinline>
        <source src="/rns_rust_version.mp4" type="video/mp4" playsinline>
    Your browser does not support the video tag.
    </video>
    <p>This is the updated rust code, execution time=1.18s</p>
</div>

Here are some contour plots that have been generated in rust using the `plotly` crate. The plots are generated as `html` images which have been converted to `png` format.
<div class="row">
    <div class="column">
    <a href="/abs_velocity_OmegamaxOn2.png" target="_blank">
    <img src="/abs_velocity_OmegamaxOn2.png" title="Velocity at half maximum rotation speed"  id="img2" style="width:100%">
    </a></div>
    <div class="column">
    <a href="/pressure_OmegamaxOn2.png" target="_blank">
    <img src="/pressure_OmegamaxOn2.png" title="Pressure at half maximum rotation speed"  id="img2" style="width:100%">
    </a></div>
</div>

Overall, the rust conversion of the C program is at a point where I can compare the results. Eventually, I will write some python wrappers over the rust code so that it can be called from within python. This makes it much more useful to the astrophysics community in general.

# CPAP medical data case study: python to rust
This case study reads in binary data that has been written to an SD card by a BMC CPAP machine and parses the binary data and generates a csv output file. The original python code was cloned from <a href="https://github.com/headrotor/BMC_RESmart" target="_blank">github:headrotor/BMC_RESmart</a> and my clone is <a href="https://github.com/pauleaster/BMC_RESmart_Clone" target="_blank">here</a>. I wished to port this code to rust to see firsthand how much faster it was and to investigate the difficulty level involved in porting python to rust. As this was a much smaller project, around 300 lines of code, I did not include unit tests as I did with the RNS port. 

The binary CPAP data was around 100MB and the csv file written was around 30MB. The python code takes around 30 seconds to read the binary file, parse the csv, and write it to disk. The rust port takes around 1.5 seconds to do the same thing!!!

Here is a video that shows the relative speeds of the two programs. The python executable is ran first and the rust executable is ran after the python code finishes. 

<video width="640" height="240" controls>
        <source src="/CPAP_timing_comparison.mov" type="video/mov" playsinline>
        <source src="/CPAP_timing_comparison.mp4" type="video/mp4" playsinline> 
    Your browser does not support mov video tags.
</video>

In this instance, the python code takes 35 seconds and the rust code takes 1.9 seconds. That is a speedup of around ***18x***.

Another unintended benefit of using rust was that the csv generated by rust had eight extra lines (python csv = 481944 lines, rust csv = 481952 lines). There is a bug in the python code where a line is dropped for each read of the binary data file. Because of the memory safety, strict type checking, and compiler enforcement, the rust code did not have this bug. So the rust code is faster and safer than the python code. It should be noted that the python code was written as service to the CPAP community and there was no guarantee on the correctness of the results. Therefore, it was likely that it was not rigorously tested for correctness. On the other hand, the rust code was not rigorously tested either, what is seen here is what was developed out of the box. 
<br>
<br>
<br>
<br>
<br>
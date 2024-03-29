+++
title = "Web development"
weight = 30

date = "2021-03-12"
+++

# This website
This website has been designed using <a href="https://www.getzola.org" target="_blank">Zola static website software</a>  which has been written in the <a href="https://www.rust-lang.org" target="_blank">rust programming language</a>. It uses an extensively modified <a href="https://www.getzola.org/themes/karzok" target="_blank">Karzok template</a> and is currently served on  <a href="https://pages.github.com" target="_blank">Github pages</a>.
To get this website up and running it was necessary to code in html, css, shortcodes, and markdown.




# Web servers
An  <a href="https://httpd.apache.org" target="_blank">apache server</a> has also been configured to serve this website, though it is not currently active. Additionally, it would be a relatively simple task to serve this on  <a href="https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html" target="_blank">Amazon S3 using Amazon amplify</a>
for a server-less configuration which would eliminate a single point of failure for the server. An alternative server-less hosting paradigm  would use a light-weight server, for example <a href="https://github.com/joseluisq/static-web-server" target="_blank">static-web-server</a> , also written in rust, in a container (e.g. Docker) configuration.


# Mobile websites
Note that this website has yet to be optimised for mobile websites. This is on my `todo()` list.

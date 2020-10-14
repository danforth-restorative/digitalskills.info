---
layout: post
title: Digital transformation for the rest of us
subtitle: Each post also has a subtitle
gh-repo: digitalskills-info/digitalskills-info.github.io
gh-badge: [star, fork, follow]
tags: [org]
comments: true
---

<form method="POST" class="subscribe" action="https://digitalskills.info/staticman/v2/entry/digitalskills-info/digitalskills-info.github.io/master/subscribe">
  <input name="options[redirect]" type="hidden" value="https://github.com/digitalskills-info/digitalskills-info/pulls">
  <input name="options[slug]" type="hidden" value="restofus">
  <input name="fields[name]" type="text" placeholder="Name">
  <input name="fields[email]" type="email" placeholder="Email">
  <input name="fields[message]" type="text" placeholder="Areas of Interest">
  <center><button type="submit">Subscribe</button></center>


location /staticman {
    proxy_set_header   X-Forwarded-For $remote_addr;
    proxy_set_header   Host $http_host;
    proxy_pass         http://localhost:1111;
}
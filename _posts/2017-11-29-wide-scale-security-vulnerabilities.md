---
layout: post
title:  "Wide-Scale Security Vulnerabilities"
date:   2017-11-29 09:12:25
categories: security
tags: security macos jekyll
---
I was browsing Twitter this morning, when I came across something rather interesting. Then, just a couple minutes afterwards, I got an automated message from GitHub warning me about a security hole right here, on the blog.

These two (albeit super random occurrences) got me to finally write another blog. This is not at all an advanced security bulletin, just a heads up for everyone

## The Tweet
{% twitter https://twitter.com/lemiorhan/status/935578694541770752 %}

According to this tweet, there is a massive hole in MacOS High Sierra, allowing pretty much anyone to change or access any password protected section on your Mac.

Apple are currently playing dead, inviting people to send them DMs on Twitter, rather than making a statement. I actually kind of understand that though, this is a massive fuck up and I hope it gets dealt with soon.

I don't leave my Mac unlocked. Ever. I have a hot corner setup that triggers a screen saver, and my password is required immediately.

Ironically enough, I changed all these settings with the root exploit.

The solution to fix this issue is quite simple, just follow [these steps](https://t.co/LqNVwVvxEb) to set a PW for a root user.

## The GitHub Email

This morning, GitHub sent me an email informing me about a potential **high severity** security vulnerability. Even though this could be a scary situation, it's great that GitHub sends out warnings about these wide scale vulnerabilities.

In real life, I try to avoid these situations as much as possible. Being informed of a treat by your own repository is kind of an equivalent to a hail Mary.

## Prevention

I have my own system when it comes to using package managers like npm. First of all, thoroughly check that you're installing the **right package**. Yep, if you didn't know, there was a [high profile malicious case](https://iamakulov.com/notes/npm-malicious-packages/), where multiple packages banked on people entering the wrong package name. Once installed, they could send all your ENV values to the package owner.

**System updates**. Update and upgrade packages running on your production systems regularly. It doesn't matter if it's a large scale app deployed all over the globe, or your sister's blog. Do it.

Keep the **packages up to date**. Set up your package manager to at least grab the patch versions from the package you're currently using. 

It's always good to be proactive, rather than being reactive. Especially when it comes to your own software.
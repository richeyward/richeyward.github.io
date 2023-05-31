---
title: "Korelogic - Crack Me If You Can Challenges"
subtitle: ""
url: "/cmiyc/"
date: "2023-05-28"
lastmod: 2023-05-29
draft: false
tags:
    - korelogic
    - cmiyc
categories:
    - CTF
---

Since 2010, [Korelogic](https://korelogic.com/) have provided an annual 'Catch Me If You Can' challenge, mostly at [DEFCON](https://defcon.org/).  From what I understand, the challenge is basic in that you are provided a file with password hashes within, and the goal is to crack as much as possible within a certain timeframe.

I have always wanted to try this as I love these kinds of challenges sp I have deliberately tried to remain ignorant on how it was solved.  On the Korelogic CMIYC page there is a breakdown of what wordlists are used and rules applied but I want to do this the way it is intended so **all of this will be ignored**.  Instead, the hash file will be downloaded and parsed with zero knowledge.

## Tools
For hash cracking, there are a number of tools that can be used. Around the time of the initial competition, [John The Ripper](https://www.openwall.com/john/) was common but hashcat was also gaining traction as the emergence of using a GPU for hashing over a CPU was trending. Despite this, I have decided to use JTR for my exercise. I also am aware that JTR has rules that allow different permutations to be tried from one keyword (and even some rules generated off of this very challenge) however again, these will all be ignored.

The only tools used outside of JTR that I intend to use is home-grown scripts.

## Challenges

### 2010

The [2010](https://contest-2010.korelogic.com/) challenge is a file containing over 53k password hashes. My writeup of it can be found below:
- [Part 1](2010-p1)
- [Part 2]()


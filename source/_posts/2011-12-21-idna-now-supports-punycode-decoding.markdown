---
layout: post
title: "IDNA Now Supports Punycode Decoding"
date: 2011-12-21 19:27
comments: true
categories: Lisp
---

My [IDNA library](http://github.com/antifuchs/idna) now supports
decoding IDNA strings via the to-unicode function:

        (to-unicode "xn--mller-kva.example.com")
        ;; =>  "müller.example.com"

That's in addition to the regular encoding for unicode domain names:

        (to-ascii "müller.example.com")
        ;; => "xn--mller-kva.example.com"

Sadly, I haven't managed to get the logic for case-sensitive punycode
encoding to work yet. But fortunately, IDNA domain name encoding
doesn't require that! Anyone looking for some low-hanging fruit-shaped
lisp projects is welcome to add that! (-:

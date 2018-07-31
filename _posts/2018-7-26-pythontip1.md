---
layout: post
cover: 'assets/images/python.jpg'
title: virtualenv and ungrade
date: 2018-7-19 15:00:00
tags: python
author: sunao
---

<h2>virtualenv</h2>

<p>virtualenv is a python library that helps people in constructing a independent python environment, that is, you can handle python2.7 and python3 in the same time and don't worry about the conflict of two version of python libraries.</p>

<p>the method can be seen in this <a href="https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/
001432712108300322c61f256c74803b43bfd65c6f8d0d0000">page</a></p>

<hr />

<h2>python ungrage in linux</h2>

<p>find the exact version of python in <a href="www.python.org/ftp/python/">www.python.org/ftp/python/</a>. Then use <strong>wget</strong> to download tgz.</p>

<p>The most important step is to <strong>tag</strong> the tgz, the following paragraph will explicate the whole process.</p>

<p>at first use <strong>tar -zxvf</strong> to decompression, then use <strong>./configure </strong> to make Makefile(the specific meaning in this <a href="https://www.cnblogs.com/tinywan/p/7230039.html">page</a>). finaly use <strong>make</strong> anf <strong>make install</strong> to install.

---
layout: post
cover: 'assets/images/tencentAI.jpg'
title: The very first R package written by myself
date: 2018-7-30 20:00:00
tags: R
author: sunao
---

<p>I am writing a R interface to <a href = 'https://ai.qq.com/'>tencent AI lab</a>. Until now, I just finish the interface to chat robot and emotion analysis</p>

<p>You can install my package by running</p>
<p> <strong> devtools::install_github('suntiansheng/tencentAI')</strong></p>
<p> If you do not install devtool, you need to make sure you have installed it before run this code in your R CMD. Meanwhile you need sign up to <a href = 'https://ai.qq.com/'>tencent AI lab</a> and creat a APP</p>

<p>I just come into contact with HTTP last week, so I am not sure about the robustness of this package</p> 

<hr />

<h4>ISSUE</h4>

<blockquote>
<p> Didn't use <strong>testthat</strong> because there are four errors that I can not figure out.</p>
<p> Did't use version control because I did not find github pane in project option.</p>
</blockquote>

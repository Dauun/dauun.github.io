---
layout: post
title: "Text Formatting"
author: "Said Dauun"
categories: journal
tags: [documentation,sample]
image:
  feature: spools.jpg
  credit:
  creditlink:
---

## Introduction

As part of my javascript practice month I decided to make all the code tests in the eloquent javascript book.

### Code

FizzBuzz Solution

```js
var t = "";
for(var i = 1; i <= 100; i++){
	if(i % 3 == 0){
		t += "Fizz";
    }
	if(i % 5 == 0){
		t += "Buzz";
	}
	console.log(t != "" ? t : i);
	t = "";
}
```

Chess Board

```js
var drawLine = function(l){
	var line = "";
	for(var i = 1; i <= 8; i++){
		if(i % 2 == l % 2){
			line += "#";
		}else{
			line += " ";
		}
	}
	console.log(line);
}

for(var i = 1; i <= 8; i++){
	drawLine(i);
}
```

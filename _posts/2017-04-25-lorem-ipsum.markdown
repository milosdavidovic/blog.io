---
layout: post
title:  "Lorem Ipsum"
date:   2017-04-25 11:25:25 +0200
categories: jekyll intro
---

This is the sample page where basics of markdown syntax is demonstrated. It is a bunch of nonsense text just to show some markdown language capabilities. It is quite easy to write markdown once you get used to it.

### Content

[Page linking](#page-linking)

[Headings](#headings)

[Paragraphs and line breaks](#paragraphs-and-line-breaks)

[Italic and bold](#italic-and-bold)

[List](#lists)

[Code snippets](#code-snippets)

[Images](#images)

[Links](#links)

[Escaping special characters](#escaping-special-characters)

#### Page linking

It is possible to enable 'jumping' to content just buy referencing to the heading name.

Example:

```
[Paragraphs and line breaks](#paragraphs-and-line-breaks)
```

Result:

[Paragraphs and line breaks](#paragraphs-and-line-breaks)

#### Headings ####

There are four level of headers available in markdown (using character in front of text but separated with one space):

# First level header #

(don't know why this is so small)

## Second level header ##

### Third level header ###

#### Fourth level header ####

#### Paragraphs and line breaks ####
To have separated paragraphs all you have to do is to insert empty line between two paragraphs as shown (lorem ipsum):

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce finibus nisi ante, quis mollis tellus scelerisque non. Duis ullamcorper ultrices ipsum non eleifend. Morbi ut varius orci. Sed risus est, volutpat et fringilla sed, fringilla in tellus. Ut sit amet nunc consectetur, mollis dui non, commodo leo. Fusce vehicula mi eu lorem posuere maximus. Aenean ante est, consectetur nec massa eget, consequat facilisis ipsum. Quisque in tortor sem. Etiam aliquam vitae sapien sit amet egestas.

Donec sed dapibus sem. Fusce aliquet vulputate lorem et dignissim. Nullam eu convallis ante. Fusce eu dui lacus. Aliquam rhoncus dolor quis risus blandit posuere. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce arcu ex, consectetur id tincidunt quis, elementum quis est. Maecenas placerat congue nisi lobortis euismod. Suspendisse porta sed ipsum eu gravida. Aliquam tempor ultricies nulla a pretium. In ultrices lobortis purus, eget aliquet odio laoreet nec. Aliquam aliquet risus quis tincidunt semper. Ut facilisis, orci non tempus lobortis, diam tellus finibus nisl, vel vehicula purus massa nec ante. 

#### Italic and bold

Doing italic and bold text is also possible in markdown with help of star and underscore characters. Lets write an example sentence:
Ut iaculis id **purus** at *lacinia*. Proin nibh mi, **egestas** sit amet lacus a, *placerat pretium arcu*.

#### Lists
Markdown includes support for ordered and unoredered lists:

Animals list (unordered):
* Carnivores
    * Tiger
    * Shark
    * Croc
* Ominvores
    * Elephant
    * Gazelle

Todo list (ordered):
1. Learn markdown
2. Write blog 
3. ?
4. Profit

#### Code snippets

Code snippets are written using back-tick character:

`int i = 0;`

For multiline snippets, three back-ticks are used:
```
for(int i = 0; i < 10; i++)
{
    Console.WriteLine("Viva la markdown!");
}
```
Example:

#### Images

To insert image into blog post following syntax is used:

Example:
```
![minion](https://octodex.github.com/images/minion.png "Minion")
```
![minion](https://octodex.github.com/images/minion.png "Minion")

#### Links

Also you can reference any content using the similar syntax:

Example:

```
[Google](http://www.google.com)
```

[Google](http://www.google.com)

#### Escaping special characters

You can escape special character using backslash in front of special character:

Example code:

```
\`int i = 0;\` 
```

Result:

\`int i = 0;\` 
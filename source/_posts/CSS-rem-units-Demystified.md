---
title: CSS `rem` units demystified!
readtime: 3
date: 2021-12-15 19:28:15
description:
tags:
---

In this article, I am going to talk about why I prefer  [rem](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units) which is a relative CSS unit over other CSS units. First of all, I like each CSS unit available and this post isn't going to ask you to ditch them all but there are certain places where you should be using `rem` and it does the job better. Let's understand what `rem` is first.

## What is `rem` ?
`rem` is a CSS unit which is relative to the `font-size` of the root element (`html` tag). The default `font-size` value for the `html` element for most modern browsers is 16px which means by default 1 `rem` equals to 16 `px`.

## Why `em` should be avoided?
There is another relative unit available in CSS called `em` which is relative to the font-size of the parent  (for typographical properties like `font-size`) and font size of the element itself (in the case of other properties like `width`). The reason to not use this could be the compounding effect we get when we have nested elements and each element has a `font-size` in `em` units. Such compounding effects can be really painful to maintain but for non-typographical properties it is not relative to parent but to itself thus it is fine and can be used.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="oNGwZLw" data-user="gurungrahul2" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/gurungrahul2/pen/oNGwZLw">
  em Compunding</a> by Rahul Gurung (<a href="https://codepen.io/gurungrahul2">@gurungrahul2</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>  
<br>

## `px` vs `em` vs `rem` !
One famous debate in web development world has been which is better, the old school `px` or other relative units `em` or `rem`. First of all, comparing relative units with absolute units would be pointless as they have their own uses. Use `px` when you want fixed width all the time, use `rem` when you want to have relative unit and use `em` when you want trouble, just kidding. There can be certain corner cases where you might want to use the `em`, `vh` or other units available. Lets talk about why and when you should use `rem`.

## Why we should use `rem`?
Three major reason why we should stick to `rem` is:
- Relative to the `root` element
- Respects the user's preferences
- `rem` values can be configured

### Relative to the `root` element
As described above, `rem` is relative to the root element thus it is easy to maintain and great for relative uses. Its hard to break things as relativity is on a single element if it breaks it breaks for all if it works it will work for all which is not the case for `em` as `1em` can mean different `font-size`s based on the parent.

### Respects the user's preferences
When a `font-size` is described using `rem` it respects the user's font size preference. In the current latest Chrome version (96.0.4664.45), this font size preference setting is in `Settings` -> `Appearance` -> `Font Size`. If you are setting your `font-size` using px then you are basically saying that you don't care about your users preferences which you should as a responsible developer. Something like `em` will also respect the user's preferences because it is also relative to `font-size` of the element. Use the example below by viewing the output and changing the `Font Size` from settings mentioned above and see only `Font size with rem` changes with the settings changed.

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="jOGLNvy" data-user="gurungrahul2" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/gurungrahul2/pen/jOGLNvy">
  CSS user preferences </a> by Rahul Gurung (<a href="https://codepen.io/gurungrahul2">@gurungrahul2</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>
<br>

### `rem` values can be configured
As already discussed `rem` is based on the `font-size` of the root element. Thus we can change that root elements `font-size` to configure `rem`. This makes it highly usable as I am going to discuss further.

## My favorite way to use `rem`.
I usually configure the value of `1rem` to be equal to that of `10px` which usually better as you can map things in your head. I am not the inventor of this trick, it has been around since `rem` came and heres a simple explanation on how we would do that.

-- write explanation --

## What if you have no idea where to start?
If you are complete beginner and want the best out of CSS units. Use `px` for layouts and images and `rem` for font size of the text. Simple and effective.

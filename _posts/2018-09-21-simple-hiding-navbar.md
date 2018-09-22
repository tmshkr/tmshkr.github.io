---
layout: post
title: "This is the Best Possible Navbar"
github: "simple-hiding-navbar"
author: "Tim"
---

As a developer and designer, I'm always looking to improve, to optimize things and make them better.
For example, here's the function I worked on for this site, which automatically hides the navigation bar at
the top of the screen when the user scrolls down:

```javascript
function autoHideNavbar() {
	let currentScrollPos = window.pageYOffset;
    //scrolling up
	if (!visible && prevScrollPos - currentScrollPos > scrollBuffer) {
		navbar.style.transform = 'translateY(0)';
		visible = true;
	}
    //scrolling down
	else if (visible && currentScrollPos - prevScrollPos > scrollBuffer) {
		navbar.style.transform = 'translateY(-100%)';
		visible = false;
	}
	prevScrollPos = currentScrollPos;
}
 ```

This makes it so that the viewer of the website can use the full screen to read
content, and if they want to navigate to another page in the site, all they have to do is scroll
up a bit.

It's based on these examples from [w3schools](https://www.w3schools.com/howto/howto_js_navbar_hide_scroll.asp)
and [CodyHouse](https://codyhouse.co/gem/auto-hiding-navigation). I took the best
of these two scripts and made a simple Vanilla JS implementation, with only the essential parts.
It uses a `visible` boolean, so that it can keep track of the navbar's state,
in order to have the `if` and `else if` codeblocks execute ***if and only if*** they
need to do something. Accordingly, it will hide or reveal the navbar by moving it
into or out of the user's viewport, only when the appropriate conditions are satisfied. 

One of these conditions is that the difference between the user's previous and current
scroll positions must be greater than the `scrollBuffer`, to ignore slower scrolling.
This makes it less sensitive to smaller movements, so that it only does something
when the user is quickly scrolling up or down, and not from a small random swipe.
In this way, it improves the user's experience by not having the navbar pop back into the
viewport when they didn't mean to scroll up, or when they only wanted to scroll up
a bit to view something earlier in the page.

The navbar is also responsive, so that the links collapse into a hamburger menu
for smaller screen sizes. Notably, most of the navbar serves as the button to toggle
it open or closed, while the functionality of the anchor tag enclosing the navbar's
title remains the same. Doing things this way makes it easier to open or close the
navbar, so that users don't have to tap precisely on the small hamburger button
(which can often be frustrating). It also makes it so that it isn't necessary to reach all the way across
the screen to open the menu when using a smartphone with the left hand.

To improve performance, `autoHideNavbar` is then passed as a callback function to `setTimeout` or
`requestAnimationFrame` with a ternary operator, every time the user scrolls,
so that it uses the appropriate method when the browser supports it.
This increases performance by throttling the script with `setTimeout` or allowing
the browser to handle the script with `requestAnimationFrame`. `window.onscroll`
executes very rapidly (once for every tiny scroll movement the user makes).
To get an idea of how many times it gets called, try
putting `console.log(currentScrollPos)` right after it's declared in the function.

Clearly, there are a multitude of things that must be considered when writing code
for the web, with the many and diverse devices and users that may visit your page.
Even a seemingly simple component, like a navbar, requires thoughtful deliberation
and execution in order to provide the best possible user experience.
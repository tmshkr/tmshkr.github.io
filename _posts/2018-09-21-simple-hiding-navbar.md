---
layout: post
title: "Simple Vanilla JS Hiding Navbar"
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
	if (!visible && prevScrollPos - currentScrollPos > 30 ||
		!visible && currentScrollPos < pageTop) {
		(!window.requestAnimationFrame) ?
		navbar.style.transform = 'translateY(0)' :
			requestAnimationFrame(function() {
				navbar.style.transform = 'translateY(0)';
			});
		visible = true;
	}
	//scrolling down
	else if (visible && currentScrollPos - prevScrollPos > 10 &&
		currentScrollPos > pageTop) {
		(!window.requestAnimationFrame) ?
		navbar.style.transform = 'translateY(-100%)' :
			requestAnimationFrame(function() {
				navbar.style.transform = 'translateY(-100%)';
			});
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
It uses the `visible` boolean, declared outside of the function, to keep track of the navbar's state,
so that the `if` and `else if` codeblocks execute ***if and only if*** they
need to do something. Accordingly, it will **not** unnecessarily execute code to hide
or reveal the navbar when it is already in the appropriate state. This way the
`if` and `else if` codeblocks execute only when the navbar needs to change,
as determined by their respective conditions.

One of these conditions is that the difference between the user's previous and current
scroll positions must be greater than a predetermined buffer, to ignore slower scrolling.
This makes it less sensitive to smaller movements, so that it only does something
when the user is quickly scrolling up or down, and not from a small random swipe.
The scroll buffer improves the user's experience by not having the navbar get in the
way when they didn't mean to scroll up, or when they only wanted to scroll up
a bit to view something earlier in the page. The buffers for scrolling up or down
are set to different values, so that it gets out of the way easily, and only
pops back into view when the user wants it to. `pageTop` is set equal to 1/4 the
height of the window, so that the navbar is always visible when scrolling near the
top of the page.

To improve performance, the animation for hiding or revealing the navbar is handled
by `requestAnimationFrame` if the browser supports it, as determined by a ternary operator.
This ensures that the CSS animation begins when the browser is ready for it.

The navbar is also responsive, so that the links collapse into a dropdown menu
for smaller screen sizes. Notably, most of the navbar serves as the button to toggle
it open or closed, while the functionality of the anchor tag enclosing the navbar's
title remains the same. Doing things this way makes it easier to open or close the
navbar, so that users don't have to tap precisely on the small hamburger button
(which can often be frustrating). It also makes it unnecessary to reach all the way across
the screen to open the menu when using a smartphone with the left hand.

Clearly, there are a multitude of things that must be considered when writing code
for the web, with the many and diverse devices and users that may visit your page.
Even a seemingly simple component, like a navbar, requires thoughtful deliberation
and careful execution in order to provide the best possible user experience.
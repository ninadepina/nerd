# Weekly Nerd Pre-CSS Day

## Speaker 1: Sophie Koonin (Building a website like it’s 1999 …in 2023)

-   Sophie Koonin is the Web Engineering Lead at Monzo.
-   Websites: [localghost.dev](localghost.dev), [sophie.omg.lol](sophie.omg.lol)

### Key points

-   "The web used to be weirder": In the past, web design embraced uniqueness and experimentation, but now everything tends to look the same.
-   Bring back the weird: Encouraging developers to embrace unconventional web design.
-   Use lots of GIFs: GIFs should always be rendered, with no consideration for reduced motion preferences. There is a secret way to make GIFs respect motion preferences.
-   Text effects: Consider using text effects like marquee (scrolling text) and blink (flashing text). However, animated text effects are not suitable for body text.
-   Gradient in texts: Apply gradients to the background of texts using the `background-clip: text`; and `color: transparent;` CSS properties.

### Additional resources

-   Webring for weird website lovers: [weirdwebsitelovers.neocities.org](https://weirdwebsitelovers.neocities.org/)

## Speaker 2: Adam Argyle (Animation with variable fonts)

-   Adam Argyle specializes in animation with variable fonts.
-   Website: [Gradient.style](https://www.gradient.style/)

### Key points

-   Lightness vs. luminance: HSL lightness and LCH perceptual lightness provide different perceptions of brightness. LCH perceptual lightness is consistent to human eyes.
-   Oklch: Refers to a code example available on CodePen by Adam Argyle that demonstrates the use of Oklch.
-   Gradient.style: Adam Argyle's website dedicated to gradients.

## Speaker 3: Changhaohan and Ergunsh (Debugging CSS with DevTools)

-   Changhaohan and Ergunsh discussed debugging CSS using DevTools.

### Key Points

-   DevTools: A debugging tool that sits between a browser's internals and a web app. It helps debug HTML, CSS, JavaScript extensions, network-related issues, and more.
-   Focus on reliable and adequate tooling for debugging UI-related issues.
-   Importance of CSS debugging: Ensuring that web pages look as intended is challenging due to the unique nature of CSS.
-   Challenges of CSS debugging: CSS lacks typical programming language features like variables, classes, inheritance, and functions. It has many properties with shorthands and implicit dependencies, making it harder to follow.

### New Features

-   CSS authoring hints: Help identify implicit dependencies by providing hints in tooltips.
-   CSS specificity hints: Determine the order of applied CSS rules, giving priority to IDs over classes.

### Efforts by Changhaohan and Ergunsh

-   Color tooling: Improving color-related debugging tools.
-   Animation timing function tooling: Enhancing tools for working with animation timing functions, including linear easing functions.
-   Container queries tooling: Developing tools to facilitate container queries.
-   Scroll-driven animations tooling: Improving debugging tools for scroll-driven animations.

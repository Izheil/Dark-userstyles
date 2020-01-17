# Quantum nox - Global dark style
This [userCSS](https://github.com/openstyles/stylus/wiki/UserCSS) is aimed to theme most sites you visit with dark colors similar to the ones used in Quantum Nox theme. This userstyle **doesn't work flawlessly on every site, nor does it intend to do so**, the main purpose is to just darken most sites you don't frequently visit (and thus finding specific userstyles for each one would be cumbersome apart from very inefficient) with an all-around dark style.

<img src="https://i.imgur.com/mbeHNQp.png">

**For pages that you DO visit frequently, you should use a site-specific style instead**, not only for better compatibility, but also for better visuals. You can do this by looking for an already made userstyle of any page you want themed in the [official userstyles page](https://userstyles.org/), or the few ones in here. With this in mind, this theme won't apply by default to sites like Google, Reddit, Twitter, Youtube, or Userstyles.org (since there are better alternatives out there that would do a better job at theming these sites that people visit more frequently), although you can change the regexp with the [method explained below](https://github.com/Izheil/Dark-userstyles/tree/master/Global%20dark%20userstyle#how-to-add-per-site-exclusions) to include them as sites to theme.

Examples of alternatives for these sites are the following:

* [Google](https://userstyles.org/styles/167419)
* [Youtube](https://userstyles.org/styles/123887)
* [Userstyles](https://userstyles.org/styles/141833)

For Reddit and Twitter there are already built-in dark themes, so there is no need for an external alternative.

The advantage of this global dark style over others is that it can be customized from stylus popup to your liking, like changing images contrast, changing the color of links, the background, selected text background color, darkening method...

<img src="https://i.imgur.com/kFoq6fY.png">

As per version 2.5.0 it will also paint scrollbars dark. You can choose wether to use the scrollbar recolor everywhere or only on the themed sites. It will only apply to themed sites by default.
<img src="https://i.imgur.com/hqwoq9n.png" title="Re-colored dark scrollbar" />

You can find the dark theme of stylus used in the image (along with other styles made by the same author) [here](https://gitlab.com/RaitaroH/Stylus-DeepDark).

[![Install directly with Stylus](https://img.shields.io/badge/Install%20directly%20with-Stylus-00adad.svg)](https://raw.githubusercontent.com/Izheil/Quantum-Nox-Firefox-Dark-Full-Theme/master/Global%20dark%20userstyle/Quantum%20Nox%20-%20Global%20dark%20style.user.css)

This global userstyle won't apply on certain sites (you can also specify your own) that already have a dark mode by default (although you can enable it for those if you want changing the regexp as specified on the section below this one).

## How to add per-site exclusions
You can exclude certain sites from the userstyle to avoid having to turn it on or off on sites you have other more specific userstyles enabled.

To do so, all you have to do is add the domain you don't want the style to apply inside the regexp line (without the http(s) header), and separate each one with a |, like it's already done for a few sites by default:

<img src="https://i.imgur.com/F2ecrCn.png">

For example, if we already have some rule for some site and **wanted to add an exclusion** for *example.com*, the new rule would look like:

<pre>
@-moz-document regexp("https?://(?!(www.nothingtoseehere.com|www.example.com)).*") {
/* Add any other site you don't want to apply inside the regexp encased between |'s ^ */
</pre> 

This would make the userstyle ignore all pages that start with *www.example.com* (as well as the *nothingtoseehere* domain before it), <b>but not the subdomains like <i>subdomain.example.com</i></b>.

**We also must be cautious to use (or avoid) the <code>www.</code> heading in the exclusion. If it appears in the URL bar when you access the site (if it says<code>https://www.example.com</code>), you will have to write it like <code>www.example.com</code> like in the previous example, but for others where the url looks like <code>https://example.com</code> you will only have to type <code>example.com</code> in the exclusion rule.**

If we only wanted to also exclude *subdomain.example.com* it would be as easy as adding only that one subdomain link to the exclusions, but **if we wanted to exclude ALL subdomains** (no matter their subdomain name) of *example.com*, we'd have to use <code>(?!w+)([^\.]+)\.</code> for all subdomain names (You don't have to specify the www part before these regexp).

That way, if we wanted to exclude all example subdomains (we assume that some subdomains may have -'s, so we need to use the complex regexp), but NOT the main domain (so we exclude all *subdomain.example.com* like pages, but not the *www.example.com* like pages) it would look like this:

<pre>
@-moz-document regexp("https?://(?!(www.nothingtoseehere.com|(?!w+)([^\.]+)\.example.com)).*") {
/* Add any other site you don't want to apply inside the regexp encased between |'s ^ */
</pre>

**If we wanted to exclude BOTH the subdomains and the domain** with just 1 selector, we would write it like <code>(((?!w+)([^\.]+)\.)?([^\.]+)\.)?</code>, otherwise we'd have to specify them apart.

This excludes both the subdomains and the domain:

<pre>
@-moz-document regexp("https?://(?!(www.nothingtoseehere.com|(((?!w+)([^\.]+)\.)?([^\.]+)\.)?example.com)).*") {
/* Add any other site you don't want to apply inside the regexp encased between |'s ^ */
</pre>

This would exclude both the subdomains and the domain of *example.com*, but wouldn't exclude other location domains like *example.org*.

To solve this and **exclude specific location domains**, you can use <code>(com|org)</code> or similar rules with all the location domains of the page. This can be used independently of the previous regexp rules. The full example so far would be:

<pre>
@-moz-document regexp("https?://(?!(www.nothingtoseehere.com|(((?!w+)([^\.]+)\.)?([^\.]+)\.)?example.(com|org))).*") {
/* Add any other site you don't want to apply inside the regexp encased between |'s ^ */
</pre>

Lastly, **if we wanted to INCLUDE some path of a domain to be affected by this, while still excluding all other pages of the domain**... we would use <code>((?!text-to-include-here).)*$</code> in the place where the text that distinguises the page we want to include would appear.
Following our previous example, let's say we wanted the theme to apply to *example.com/home*, but NOT on every other *example.com* domain or subdomain page (like *example.com/contact* or *subdomain.example.com*). We'd have to use regexp to create an inclusion for this with "home":

<pre>
@-moz-document regexp("https?://(?!(www.nothingtoseehere.com|(((?!w+)([^\.]+)\.)?([^\.]+)\.)?example.(com|org)((?!home).)*$)).*") {
/* Add any other site you don't want to apply inside the regexp encased between |'s ^ */
</pre>

## How to force color-inversion method instead of regular theming for a site
Since version 2.4.0 you can specify sites that you want to be themed with the "Color Inversion" method, which just returns the page with the complete opposite colors it has by default.
All you need to do is add a new section and specify the start of the url, or the domain of the page (depending on the option you choose).
<img src="https://i.imgur.com/ZErV54r.png">

## Settings explanation
There are a few settings that you can change clicking the gear icon inside stylus popup that might not be intuitive:

* **Highlight links**: Sometimes link coloring will fail due to the site dark link color overwritting the changes of the global style. You can turn this setting on to put a light background color behind links for better visibility.
* **100% bright img on hover**: It will make images show as their default contrast when you hover over them (won't do anything unless you've set **Image brightness %** to anything lower than 100).
* **Image brightness %**: Will change the brightness of images tagged as <code><img></code>, so banners, header images, and other div background images will still show as default even if you change this setting. An example of images affected would be any that you can find inside posts of most social media sites, and possibly their logo. An exaple of the ones that WON't be affected would be the images that appear as the background of banners that common visual-oriented sites use (like [GIMP](https://www.gimp.org/), or [Apple](https://www.apple.com)).
* **Invert image colors**: Shows the negative version of the image, while trying to respect non black/white colors.
* **Apply on div img background**: Applies the image brightness or color inversion changes (if enabled through the other settings) on divs that have an obvious background image (doesn't work with all elements).
* **Hide div background images**: Hides the background images of elements that aren't headers, nor post images. An example would be forums that use a gradient image for the post body of the threads, showing white over the whole post background. It also can hide certain images that act as banners, but won't hide all banners.
* **Hide header images**: Hides background images that act as headers and banners (like the examples provided on image brightness description).
* **Hide images of links**: This will hide all background images of links that have one, including certain buttons that are tagged as links.
* **Lower header img contrast**: This will try to lower the brightness of banners and header images while still showing their background (if hiding header images option is enabled, this one is not necesary).
* **Dark layers over divs**: This will paint semi-transparent dark layers behind most elements to attempt to lower the contrast of some elements that still have background images. You shouldn't need this unless you want to see the background images of divs (rare occasions).

## Donations
If you want to support this theme, consider buying me a coffee to motivate me keep this repository up and running.
​
<a href='https://ko-fi.com/K3K4TQ97' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
## Moving stylesheets forward dynamically
When I started 5 years ago, the university had a myriad of small stylesheets governing how each section — sometimes down to a single page — looked, on average these were small files and shared a lot of rules, generally due to one being copied as the starting point of the next.

The team soon embarked on a huge project to create a standard, consistent visual identity and to ensure this was applied correctly and easy to maintain we created one “global” stylesheet to replace all the narrow-scoped ones.

This global stylesheet has been modified and added to over the last half-decade as more demands are made of it, until it now weighs in at over 1600 lines of rules. We’ve often discussed ways to make this file easier to work with and update, usually around breaking it up into smaller, more specific files that were then brought back together for publishing. There just wasn’t an elegant way of doing this at the time. It was also apparent that several attributes (particularly colours and typography) were repeated throughout the document, and possibly overwritten further down as new rules were appended.

This really frustrated the team as we knew there should be a better way of doing this.

As part of relaunching our Research section we wanted to move from plain, hand-typed stylesheets to something more powerful, flexible and easy to maintain — which meant looking at either SASS or LESS as CSS preprocessors, which also meant we could address the issues of scalability and repetition that had started to bog down our global stylesheet.

We went with SASS as it was a better fit with our existing infrastructure, and I personally found it easier to pick and integrate into my workflow. Over the next few weeks I’ll post about the lessons we’ve learnt from using SCSS in the wild, but first I thought it’d be good to share some of the key reasons for adopting this approach to creating CSS files.

### Configuration
A simple ruby file allows you to customise the configuration of your project, and gives your processor (we used compass locally, and scssphp remotely due to current lack of a ruby dev server). This meant we could define the folder structure and also the level of commenting and compression applied to the output scss files. It also meant we could have separate dev and production config files:

Our defined structure — we have a preference for a HTML-based naming convention to files, so out went “stylesheets” “images” and “javascript” in favour of “css”, “img”, and “js”. We also added an underscore prefix to the default sass folder to ensure it always appeared at the top of any directory.

### Variables
Define once, reference often. Prior to having custom defined variables we’d have to manually re-enter the same information over possibly dozens of rules, and then search and replace them whenever a change to the presentation was required. A great example of this would be defining font-stacks and global colour schemes:

```$headings: 'Raleway', sans serif;
$copy: 'Lucida Sans', 'Lucida Grande', 'Lucida Sans Unicode', 'Bitstream Vera Sans', sans-serif;
$interaction: AlexandriaFLF, "Palatino Linotype", Palatino, Georgia, "Times New Roman", serif;

$linkcolour: #ee3388;
$bodycolour: #404040;
```
This could now be dropped into rules, safe in the knowledge that future iterations would only require one update in one place.

### Partials
A simple but powerful benefit of using scss is that you can create partials — files of small specific rules that can be included into larger files at compilation. Our current list of partials gives an indication of the purpose of each — meaning simpler debugging, and also the ability for several people to work on areas of our site without having to constantly commit changes to the same main scss file and then compile remotely.

 -_normalize — a standard set of rules to remove differences between browser rendering. Based on the file that ships with sass builds of Foundation 4.
 -_baselayer — a set of rules to apply common presentation elements, all our basic visual styling is here.
 -_colour — predominantly the variables for our current global colour palette
 -_typography — again, mostly variables and functions

### We still have a lot of work to do.
Okay, so that’s a quick up to speed of why we made the switch, and where we’re up to. As mentioned, I’ll be writing posts whenever we have significant discoveries, decisions, and/or solutions to share — for instance the following are all currently up for debate:

- Agreeing naming conventions
- How best to break up our stylesheets using partials
- Being able to compile remotely and locally in the same fashion
- Optimising existing CSS into SCSS
- Deciding what (if any) mixin libraries to @include — compass and particularly bourbon look interesting.
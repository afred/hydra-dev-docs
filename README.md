# Enhancing Hydra Developer Documentation

This project was started as an effort to make some improvements to the developer documentation for Hydra that were identified at the Hydra Partners meeting at Stanford in March 2013.

These include, but are not limited to:

- Update list of knowledge required
- Identify and nuke stale docs
- Version docs with code
- (?) Have own repository for documentation
- Tips, pointers, best practices
- Migrate developer documentation from Duraspace to git hub
  - communicate the move
  - request time boxed approval
- Documentation audit and automation at Boston mtg (1/2 day)
- Module of what I need to know as a manager

**NOTE:** While Hydra is comprised of many different projects working together, this effort is focused primarily on the **hydra-head**.

### Main Objectives for this effort

- Review original pages from the hydra-head wiki, and the Duraspace wiki.
- Combine duplicate content
- Organize content such that places to put new, relevant content are obvious.
- Remove migrated content from Duraspace wiki, referencing the new location.

### Original pages from Hydra-Head Github Wiki

The original wiki files from https://github.com/projecthydra/hydra-head/wiki have been saved down in this repo at https://github.com/afred/hydra-dev-docs/tree/master/hydra-head-wiki-orig. We can use these as a starting point

Also, the wiki for _this_ repo started as a clone of https://github.com/projecthydra/hydra-head/wiki, but should serve as a sandbox for getting the wiki to an improved state.

### Original Pages from Duraspace wiki.

One of the objectives above, is to migrate developer docs from https://wiki.duraspace.org/display/hydra/Developers to the hydra-head wiki at https://github.com/projecthydra/hydra-head/wiki.

I grabbed all pages I could find under the "Developers" part of the wiki, and put them in this repo under https://github.com/afred/hydra-dev-docs/tree/master/duraspace-wiki-orig/Developers. The html file there are essentially the markup found in the `<div id="main-content" class="wiki-content">` element of each page, with page titles added manually after the fact.

**NOTE:** There may be other relevant docs from the Duraspace wiki intended mainly for Developers which should be identified, and included in this migration.

#### My steps for converting Duraspace wiki pages to github markdown.

I'm using:
- Chrome
- Sublime Text 2
- Pandoc plugin for Sublime to convert from HTML to MD.

1. Navigate to the desired page.
1. Get the markup you're after, the stuff with the actual content.
  1. using chrome, right click to 'Inspect Element'
  1. Find the html node containing the content
  1. right click and select 'Copy as HTML'
1. Paste into Sublime and save it with `.html` extension.
1. `Cmd+P` to open your list of available packages, and select Pandoc.
1. The list will change to available formats, select Markdown. This will create a new file with the markdown. Save it with `.md` extension.
1. Prepend relative URLs in parentheses with root URL of duraspace wiki
  1. Use this regex to find (most of) them: `\(\/([a-zA-Z0-9\/_\-\+:\/\.%]+)\)`
  1. And replace with this: `(https:wiki.duraspace.org/\1)`. For me, `\1` is the variable with my first match from the regex.
1. Remove the wiki's table of contents anchors, that look something like this `{#Name-of-the-anchor}`
  1. this regex will find (most of) them: `*\{([a-zA-Z0-9:#\-\?_\+&"-\(\)]*)\}` (note the prepended space(s))
  1. And just replace them with empty string.
1. Pandoc puts trailing backslashes in for some reason, so remove those.
  1. this regex will find them: `\\$`
  1. and just replace them with empty string.
1. And there are probably some one-off stuff too. This is just to get us close.
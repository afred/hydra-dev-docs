## How I'm converting pages from duraspace wiki to markdown

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
  1. Use this regex to find them: `\(\/([a-zA-Z0-9\/_\-\+:\/\.%]+)\)`
  1. And replace with this: `(https:wiki.duraspace.org/\1)`. For me, `\1` is the variable with my first match from the regex.
1. Remove the wiki's table of contents anchors, that look something like this `{#Name-of-the-anchor}`
  1. this regex will find them: ` *\{([a-zA-Z0-9:#\-\?_\+&]*)\}` (note the prepended space(s))
  1. And just replace them with empty string.
1. Pandoc puts trailing backslashes in for some reason, so remove those.
  1. this regex will find them: `\\$`
  1. and just replace them with empty string.
# Hydra Developer Documentation: Review, Consolidate, Organize.

This project is an effort to make some improvements to the developer documentation for Hydra that were [identified at the Hydra Partners meeting at Stanford in March 2013](https://wiki.duraspace.org/display/hydra/Documentation+Framework+Discussion+at+Hydra+Partners+-+March+2013).

### Objectives

- Review, consolidate, and organize content from the [hydra-head wiki](https://github.com/projecthydra/hydra-head/wiki), and the "for Developers" section of the [Duraspace wiki](https://wiki.duraspace.org/display/hydra/Developers).
- Consolidated and organized content goes in [the wiki for this project](https://github.com/afred/hydra-dev-docs/wiki).
- Archive content from Duraspace Wiki in ["Archived Developer Documentation"](https://wiki.duraspace.org/display/hydra/Archived+Developer+Documentation).
- Add steps for versioning the wiki with the code in documentation about cutting releases.

###### What we _don't_ want to do right now

- Refactor pages meant for non-developers from the Duraspace Wiki.
- Refactor github wikis for any other gem besided `hydra-head`.


### Overall Workflow

Here's is the general workflow. See sections below for further details:

1. Identify docs from hydra-head wiki, or Duraspace wiki that need addressed.
1. Create a ticket in github (a.k.a. "issues") to review, consolidate, and/or organize the content.
1. Work the ticket.
  - If we're keeping the content, add it to [this wiki](https://github.com/afred/hydra-dev-docs/wiki).
  - Archive old page(s), if applicable.
1. Close the ticket. Do a little dance.

##### When are we done?

The content from the [this wiki](https://github.com/afred/hydra-dev-docs/wiki) will be consolidated into hydra-head wiki proper after approval by the community. This approval process is TBD, will involve a call to action announced on the [hydra-tech mailing list](https://groups.google.com/forum/?fromgroups#!forum/hydra-tech) at the very least.

### Creating Tickets

We're using Github to track progress on reviewing, consolidating, and organizing docs.

1. Check the [github issues](https://github.com/afred/hydra-dev-docs/issues) to see if there is already a ticket for the documents. If not, then create a new ticket.
1. The **Title** field should include the name of the document being addressed, and a brief description of what needs to be done, e.g. **Include content from Duraspace wiki page, "Guidelines For Committers", and archive the original.**
1. Include the following info (if applicable) in the **Comments**:
  - where the content should be included in the new wiki
  - link to original document or archived location
  - if only some of the content was included, briefly mention what was kept and what was not.

### Archiving old pages from hydra-head github wiki

We are in the process of implementing a workflow to clone the hydra-head wiki and save it in the hydra-head repo (location TBD). Once this is in place, archiving the wiki with the code will be inherent in the release process.

### Archiving Duraspace Wiki pages

Instead of nuking the Duraspace wiki pages, we have decided to:

1. mark them with a deprecation message warning
1. move them to [Archived Develper Documentation](https://wiki.duraspace.org/display/hydra/Archived+Developer+Documentation)

##### To add the deprecation warning to Duraspace wiki pages:

1. Go to the page you wish move to "Archived Developer Documentation".
1. Click the 'Edit' icon (top right).
1. Open the source editor by clicking the button labeled `<>` (top right).
1. Prepend the following markup in the source editor, and click 'Apply' (bottom left)

    ```html
    <ac:macro ac:name="warning">
      <ac:rich-text-body>
        <p>WARNING: This content has been deprecated!</p>
        <ac:macro ac:name="highlight">
          <ac:parameter ac:name="atlassian-macro-output-type">BLOCK</ac:parameter>
          <ac:rich-text-body>
            <p>Please browse or search the main wiki for more up-to-date content.</p>
          </ac:rich-text-body>
        </ac:macro>
      </ac:rich-text-body>
    </ac:macro>
    <p> </p>
    ```

1. View the change by clicking button labeled 'Preview >'. The page should have a red box with the deprecation warning in it just below the page title.
1. If the deprecation warning looks good, click 'Save'.

##### To move Duraspace wiki pages to "Archived Developer Documentation":

1. Go to the page you wish move to "Archived Developer Documentation".
1. From the **Tools** menu (top right), select **Move**.
1. In the box labeled **New parent page:** type "Archived Developer Documentation". It should show up as one of the options as you type.
1. Click **Move**.

### Other Notes

##### Wiki pages saved in this repo

For convenience, content from both wikis has been saved to this repo (as of ~ 6/3/2013):

- [Duraspace pages (converted to markdown)](https://github.com/afred/hydra-dev-docs/tree/master/duraspace-wiki-orig)
- [hydra-head wiki pages](https://github.com/afred/hydra-dev-docs/tree/master/hydra-head-wiki-orig)

##### Converting Duraspace Wiki pages to markdown.

- When viewing the source of a Duraspace wiki page, the main content is contained within
    
    ```html
    <div id="main-content" class="wiki-content">
      ...
    </div>
    ```

- I use Sublime Text 2 + a package called Pandoc to convert html to markdown. Pandoc is pretty rad.
- Helpful regular expressions:
  - `\(\/([a-zA-Z0-9\/_\-\+:\/\.%]+)\)` -- helps find relative URLs inside of parentheses.
  - `*\{([a-zA-Z0-9:#\-\?_\+&"-\(\)]*)\}` -- helps find table of contents anchors
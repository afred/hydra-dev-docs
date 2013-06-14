# Hydra Developer Documentation: Review, Consolidate, Organize.

This project is an effort to make some improvements to the developer documentation for Hydra that were [identified at the Hydra Partners meeting at Stanford in March 2013](https://wiki.duraspace.org/display/hydra/Documentation+Framework+Discussion+at+Hydra+Partners+-+March+2013).

## Objectives

- Review, consolidate, and organize content from the [**hydra-head wiki**](https://github.com/projecthydra/hydra-head/wiki), and the "for Developers" section of the [**Duraspace wiki**](https://wiki.duraspace.org/display/hydra/Developers).
- Consolidated and organized content goes in [**the wiki for this project**](https://github.com/afred/hydra-dev-docs/wiki).
- Archive content from Duraspace Wiki in ["**Archived Developer Documentation**"](https://wiki.duraspace.org/display/hydra/Archived+Developer+Documentation).
- Add steps for versioning the wiki with the code in documentation about cutting releases.
- Create a set of consistent documenation practices for other gems within projecthydra that are:
    - maintainable
    - navigable
    - versionable

### What we _don't_ want to do right now

- Refactor pages meant for non-developers from the Duraspace Wiki.
- Refactor github wikis for any other gem besided `hydra-head`.

## Workflow

Use Github to track progress on reviewing, consolidating, and organizing docs.

### Adding Tickets

1. Check for [**existing tickets**](https://github.com/afred/hydra-dev-docs/issues?state=open) first.
1. Make the ticket searchable by including things like the original wiki page title, keywords, etc.
1. Make the ticket filterable by applying appropriate milestones and/or labels.
1. Clearly state what needs to be done in order to close it.

### Working Tickets

1. All changes should be made on [**the wiki for _this_ project**](https://github.com/afred/hydra-dev-docs/wiki), and **_NOT_ the hydra-head wiki**.
1. Follow the [**Wiki Conventions**](#wiki-conventions) outlined below.
1. Changes will be pushed to the hydra-head wiki after thumbs up from the community.

_"Thumbs up from the community" is TBD, pending community feedback._

### Editing Exsiting Pages

Feel free to edit the pages [**directly from Github**](https://github.com/afred/hydra-dev-docs/wiki).

### Adding New Pages, Reorganizing Pages

You will need to clone the wiki repo to your local machine, edit, and push back to Github.

```bash
git clone git@github.com:afred/hydra-dev-docs.wiki.git
```

#### Using Gollum locally

Github wikis are powered by Gollum, which is free to install, and gives you an editor that you can run in your browser that is similar to what you see on Github. (NOTE: They are not exactly the same. Differences are mentioned in the [Wiki Conventions](#wiki-conventions) section below).

[**Instructions for installing and running Gollum**](https://github.com/gollum/gollum/).


## Wiki Conventions

_Subject to change pending community feedback._

### Filenames and Page Titles

Github displays a page title that is derived from the filename. Dashes are replaced with spaces, and case is unchanged. So keep that in mind, and name files such that they make pretty page titles.

**Examples:**

- `i-am-lowercase.md` will have a page title of **`i am lowercase`**
- `i_have_underscores.md` will have a page title of **`i_have_underscores`**
- `Pretty-Page-Title.md` will have a page title of **`Pretty Page Title`**
- `some_dir/I-am-in-a-Directory.md` will have a page title of **`I am in a Directory`** (github ignores the path when displaying the page title)

##### Avoid renaming files, or changing page titles

Changing page titles or filenames tends to break links due to Github's implicit relationship between filenames and page titles.

##### If editing from Github:

- **Do not try to move a page to a different directory.** Github doesn't allow this. You have to clone the repo to your local machine and do it from there.
- **Avoid creating new pages.** Github doesn't allow you to organize files into directories; it saves everyting to root dir of repository.

##### If editing locally with Gollum:

- **Don't override the page title with H1 at the top of the page.** Gollum allows this by default, but Github ignores it.
- **Don't worry if it displays directory in page title.** Github will ignore the directory, and only use the filename to derive the page title.

### Directories

Github sort of ignores directories in the wiki repository. But there are a couple of advantages to organizing wiki pages into directories:

1. You can take advantage of Gollum's `_Sidebar.md` files for navigation, which helps with maintainability and consistency.
1. The structure is preserved when the wiki pages are copied to the main repo for versioning, which helps keeping old documentation navigable.

**NOTE:** Filenames still must be unique, even if they are in different directories.

##### Guidelines for Directories:
1. Create a new directory when a group of related content has grown to span multiple page, or needs to be broken into multiple pages.
1. Name the directory something relevant, using lowercase letters and underscores, e.g. `getting_started/tutorials/dive_into_hydra/`.
1. Each directory should have a main page that is the same name as the directory, only with a Github-friendly filename, e.g.:
  - `getting_started/tutorials/Tutorials.md`
  - `getting_started/tutorials/dive_into_hydra/Dive-into-Hydra.md`.
1. Each directory should contain a `_Sidebar.md` for navigation, e.g.:
  - `getting_started/tutorials/_Sidebar.md`
  - `getting_started/tutorials/dive_into_hydra/_Sidebar.md`
1. When linking to files inside of directories using markdown's double brackets, you do not need to include the path, e.g.:
  - **BAD:** `[[getting_started/tutorials/Tutorials]]`
  - **GOOD:** `[[Tutorials]]`

### Navigation with `_Sidebar.md` files.

Special files named `_Sidebar.md` are used for navigation ([see how they work](https://github.com/gollum/gollum/wiki#sidebar-files)).

This makes maintaining links easier, since `_Sidebar.md` will be displayed for all pages in the same directory.

##### Guidelines for `_Sidebar.md` navigation:
1. There should be one for each directory.
1. Should contain a link to main page of parent directory.
1. Should contain a link to every page in the directory.
1. Should contain a link to main page for every child directory.

## Archiving old pages from hydra-head github wiki

We are in the process of implementing a workflow to clone the hydra-head wiki and save it in the hydra-head repo (location TBD). Once this is in place, archiving the wiki with the code will be inherent in the release process.

## Archiving Duraspace Wiki pages

Instead of nuking the Duraspace wiki pages, we have decided to:

1. mark them with a deprecation message warning
1. move them to [Archived Develper Documentation](https://wiki.duraspace.org/display/hydra/Archived+Developer+Documentation)

### To add the deprecation warning to Duraspace wiki pages:

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

### To move Duraspace wiki pages to "Archived Developer Documentation":

1. Navigate to the page to be archived, and from the **Tools** menu (top right), select **Move**.
1. In the box labeled **New parent page:** type "Archived Developer Documentation". It should show up as one of the options as you type.
1. Click **Move**.

### If you need to convert Duraspace wiki page to markdown...


- When viewing the source of a Duraspace wiki page, the main content is contained within
    
    ```html
    <div id="main-content" class="wiki-content">
      ...
    </div>
    ```

- I use Sublime Text 2 with [**Pandoc**](https://github.com/tbfisher/sublimetext-Pandoc) to convert html to markdown. Pandoc is pretty rad.
- Helpful regular expressions:
  - `\(\/([a-zA-Z0-9\/_\-\+:\/\.%]+)\)` -- helps find relative URLs inside of parentheses.
  - `*\{([a-zA-Z0-9:#\-\?_\+&"-\(\)]*)\}` -- helps find table of contents anchors
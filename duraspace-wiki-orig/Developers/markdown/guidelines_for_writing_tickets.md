#### The Basic Idea: Cucumber-style stories {#Guidelinesforwritingtickets-TheBasicIdea:Cucumber-stylestories}

If possible, use [Gherkin](http://github.com/aslakhellesoy/cucumber/wiki/Gherkin) syntax to write [Cucumber](http://cukes.info/)-style stories in your ticket descriptions. Cucumber uses the uses Gherkin's [Given-When-Then](https://github.com/aslakhellesoy/cucumber/wiki/Given-When-Then) format to define steps within a given scenario.

*When in doubt*, at very least, include a section in your Story/Bug that says "You will know this is working when..." or "You will know this is fixed when..." 

This is an example of a Cucumber feature/story:

*We don't expect your tickets to be this detailed. See the Examples section below for links to real "story" and "bug" tickets.*

[?](#)

`Feature: Create Asset or Dataset Split Button`{.java .plain}

`  `{.java .spaces}`In order to create `{.java .plain}`new`{.java .keyword} `Assets or Datasets`{.java .plain}

`  `{.java .spaces}`As an editor`{.java .plain}

`  `{.java .spaces}`I want to see a button that will let me create a `{.java .plain}`new`{.java .keyword} `Article or Dataset`{.java .plain}

 

`  `{.java .spaces}`Scenario: Editor views the search results page and sees the add article button`{.java .plain}

`    `{.java .spaces}`Given I am logged in as `{.java .plain}`"archivist1"`{.java .string}

`    `{.java .spaces}`Given I am on the base search page`{.java .plain}

`    `{.java .spaces}`Then I should see `{.java .plain}`"Add an article"`{.java .string} `within the `{.java .plain}`"create asset"`{.java .string} `split button`{.java .plain}

`    `{.java .spaces}`And I should see `{.java .plain}`"Add a dataset"`{.java .string} `within the `{.java .plain}`"create asset"`{.java .string} `split button`{.java .plain}

 

 

`  `{.java .spaces}`Scenario: Non-editor views the search results page and does not see the add article button`{.java .plain}

`    `{.java .spaces}`Given I am on the base search page`{.java .plain}

`    `{.java .spaces}`Then I should not see `{.java .plain}`"Add an article"`{.java .string}

`    `{.java .spaces}`And I should not see `{.java .plain}`"Add a dataset"`{.java .string}

#### Examples {#Guidelinesforwritingtickets-Examples}

Examples of well written Stories:

-   [HYDRA-287](https://jira.duraspace.org/browse/HYDRA-287): [Name fields should auto-update after a computing id is entered](https://jira.duraspace.org/browse/HYDRA-287)
-   [HYLIBRA-13](https://jira.duraspace.org/browse/HYLIBRA-13): [Release this work for UVA Circulation](https://jira.duraspace.org/browse/HYLIBRA-13)

Examples of well written Bugs:

-   [HYDRA-282](https://jira.duraspace.org/browse/HYDRA-282): [Uploaded file does not appear in browse view](https://jira.duraspace.org/browse/HYDRA-282)


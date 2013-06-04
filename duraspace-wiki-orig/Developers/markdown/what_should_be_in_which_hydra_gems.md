# What Should Be in Which Hydra Gems?

[Gems](#WhatshouldbeinwhichHydragems-Gems)

-   [hydra-rails (will replace hydra-head)](#WhatshouldbeinwhichHydragems-hydra-rails(willreplacehydra-head))
-   [hydra-access-controls](#WhatshouldbeinwhichHydragems-hydra-access-controls)
-   [hydra-submission-workflow](#WhatshouldbeinwhichHydragems-hydra-submission-workflow)
-   [hydra-mods](#WhatshouldbeinwhichHydragems-hydra-mods)
-   [hydra-rails2-remnants](#WhatshouldbeinwhichHydragems-hydra-rails2-remnants)
-   [hydra-bootstrap ???](#WhatshouldbeinwhichHydragems-hydra-bootstrap???)
-   [hydra-sample-app ???](#WhatshouldbeinwhichHydragems-hydra-sample-app???)

# Gems

## hydra-rails (will replace hydra-head) {#WhatshouldbeinwhichHydragems-hydra-rails(willreplacehydra-head)}

Stuff that definitely belongs in hydra-rails gem (formerly hydra-head) – stuff you need to build rails apps that use Fedora, Solr & ActiveFedora

-   [Hydra Rake Tasks](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.ybnkrxpapy7e)
-   [Hydra Official Object & Datastream Models](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.v2pwa074jsva)
-   [Hydra FileAssets & Datastreams Controllers](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.vu279oqtcwfu)
-   Bare minimum of view helpers

## hydra-access-controls

Classes & Helpers for gated discovery, controlled access to views, and permissions-based UI features

-   [Hydra Access Controls](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.l1cp2tu648y4) 

## hydra-submission-workflow

-   [Hydra Submission Workflow](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.1r9zt9k2ls7d)  -- *currently relies on old AssetsController* *in hydra-rails2-**remnants* *gem**!*

## hydra-mods

MODS Stuff -- *currently **relies on old AssetsController in hydra-rails2-remnants gem!*

-   [Hydra Mods Article Submission/Editing UI (End-User)](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.ao6nhsdzhs4) -- should be done as view helpers as much as possible...
-   [Hydra MODS Object & Datastream Models](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.p9amw9sqy0mf)

## hydra-rails2-remnants

Legacy Stuff (mostly UI) that's used by Hydra Heads that were originally built with rails2 hydra code.  

-   [Hydra View Helpers](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.nk8f2qfu1au8) (other than the ones actually necessary for hydra-rails gem or other gems)
-   [Hydra UI/Layout (End-User)](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.lmm7r3kufg70) [all those overrides to Blacklight layout]
-   [Hydra Javascript (End-User)](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.jnazvgfmxabk) -- (other than the ones actually necessary for hydra-rails gem or other gems)
-   [Hydra CatalogController and AssetsController (deprecated)](https://docs.google.com/a/yourmediashelf.com/document/d/1q7jnOPhCQY1m7udCbAtSh72gTZzNeTUEgzx8fQVfIco/edit#heading=h.yp1o7xtpo3sx) -- should only be removed if/when downstream dependencies no longer need it

## hydra-bootstrap ???

Posible gem to create a "starter" app with decisions made up to the level of views, forms, submission workflow, etc.

## hydra-sample-app ???

Possible "Sample Rails App" gem/plugin 

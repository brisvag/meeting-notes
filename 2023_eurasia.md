## 2023-12-12
- #6439 live review

input data type is uint8

1 -> white
5 -> blue
7 -> blue
None -> purple
0 -> transparent

data = [0, 1, 5, 2, 7, 0]

expected display = [transparent, white, blue, purple, blue, transparent]

data to expected array / texture cache = [transparent, white, purple, purple, purple, blue, purple, blue, purple...]

data to texture = {None: 0, 0: 1, 1:2, 5:3, 7:3}
texture to display = {0: Purple, 1: transparent, 2:white, 3: blue}

data-texture = [1, 2, 3, 0, 3, 1]
[transparent, ...]

cache indices / np.arange(8)  = [0, 1, 2, 3, 4, 5, 6, 7]
np.arange(8) to texture / cache content = [1, 2, 0, 0, 0, 3, 0, 3]




## 2023-11-29

- [GB] next steps to 0.4.19
- [DDP] PSA [Plugins menu](https://github.com/napari/napari/pull/4991#pullrequestreview-1754589011) conversion to app-model is almost ready. Ashley and I have reviewed very thoroughly, but if folks want to pull down the branch and play with the plugins menu with various plugins, that would be handy!
- [DDP] Deprecating npe1 and menu contributions
    - Plugins menu PR has been written with the assumption that non-shimmable npe1 plugin support will be deprecated at the same time as this PR is released
    - We are "waiting" on NAP-6 to be implemented before we can deprecate non-shimmable npe1 plugins, and Robert Haase would like warning before this happens.
    - This means Plugins menu PR and NAP-6 implementation should be released at the same time
    - The current plan therefore is:
        - merge plugins PR when it is ready
        - open PR updating NAP-6 and changing its status to `Approved`. This PR will include a link for a "live demo". Tag Robert in this PR, alerting him that once this PR is merged, the next napari release will drop support for non-shimmable npe1 plugins
            - perhaps we can make use of the napari hub mailing list to alert plugin developers as well
        - simultaneously, open draft PR to napari implementing the menu contributions
        - approve & merge NAP-6
        - finish implementation
        - release major version of napari with menu contributions and app-model menus
- [DDP] The long long road to command palette
    1. Define minimal steps required for releasing app-model menus
    2. Define roadmap for converting all other actions to app-model (layer controls etc.) and full shortcut editing etc. This should include blessed ways for users to add new keybindings and simple actions
    3. Roadmap for removing action manager? (if implementing step 2 is insufficient)
    4. Once action manager is fully removed, we are ready for general command palette

## 2023-11-15

- WM: feature branches
    - Grzegorz: what is required is something like napari lite in order to implement big roadmap items. Feature branches won't prevent things being broken if not working on it for a while
    - WM: Agree on what Gregorz says, but small PRs might break something for a moment, which can't be allowed on main. Particularly with multichannel this happens all the time. Feature branches would allow for smaller PRs instead of another big PR.
- Grzegorz: blockers for release
    - direct coloring
    - labels shuffling

## 2023-09-20

- Grzegorz: [#6211](https://github.com/napari/napari/pull/6211) - upgrade dependecies workflow
    - Required because the current workflow only works on main, so it's impossible for PR devs to check dependencies, and it's a lot of work for core devs
    - One issue is reviewing the interface for the user and the other is reviewing the implementation
    - You can use the PartSeg repo to open PRs and check if the comments work as expected
- Cookiecutter:
    - We should explore Genevieve's PR for using copier instead (even if we end up moving to new repo rather than using the cookiecutter one) https://github.com/napari/cookiecutter-napari-plugin/pull/165
    - We should upgrade the cookiecutter setup files to only use pyproject.toml if possible - some developers are struggling with the python packaging mess https://github.com/napari/cookiecutter-napari-plugin/issues/167
    - Draga opened a PR " Add visibility and categories to manifest and remove .napari-hub/config.yml file #166" https://github.com/napari/cookiecutter-napari-plugin/pull/166
    - Container example: Draga plans to open a PR with a magicgui container example tomorrow, keep an eye out for it!
    - Gen also has an old PR open, we should probably get that merged: " Bugfix: check for underscores in plugin name #164" https://github.com/napari/cookiecutter-napari-plugin/pull/164
- Manifest issue in napari repository: [LINK](https://github.com/napari/napari/issues/6227)
    - we've noticed there's some duplication in the manifest and some other not very wieldy ways to declare things
    - this issue tracks any of these annoyances so that we can take a look at how it can be improved

## 2023-09-06

- Draga: [#4865](https://github.com/napari/napari/pull/4865#pullrequestreview-1608402118) file menu app-model conversion
    - Ashley had a look, Draga has had multiple looks and done a final review
    - will merge before end of the week but just want to make sure anyone who wants to look at it, has looked at it and is aware it's going in
    - Not really tested for npe1, but npe2 is passing. There is no preference for supporting npe1 fully with the app model conversion. Would be nice if people still could have an additional look. Ashley Anderson reviewed it.
    - Gregorz: would be good to have an evented keybinding system as currently collisions can occur as only for things in settings > preferences collissions are checked.
    - Gregorz: perhaps would be good to create a shortcut editor.
- WM: contrast limits

## 2023-08-22

Present: Draga, Gregorz, Lucy, WM

- WM: [#6178](https://github.com/napari/napari/pull/6178) Layer data events
    - [WM] The PR aims to emit data before and after `layer.data` gets edited. When required it is able to distinguish between drawing actions in the gui and adding a shape through `layer.add` by making use of a gui argument in `mouse_shapes_bindings`
    - [WM] What should the behaviour be when setting the data
    
        1. Keep as is (`ActionType.CHANGED`)
        2. Emit `ActionType.ADDING` and `ActionType.ADDED` if `layer.data` was not populated. Else emit `ActionType.CHANGING` and `ActionType.CHANGED` or if replaced with empty array or list emit `ActionType.REMOVING` and `ActionType.REMOVED`. 

    - [WM] Question regarding `data_indices` in `data.setter`, what should be emitted?
    - [Draga] In case of REMOVED, empty data indices as the data indices that are removed can be picked up with REMOVING.
    
- Draga & Lucy: Plugin menu and menu contribution 
    - Documentation states that magicgui factory, subclasses of QWidget and additionally a wrapper function that returns a widget instance are supported with regards to plugin widgets. Unclear what is truly supported.
    - Grzegorz confirms only magicgui factory and subclasses of QWidget are supported.
    - Grzegorz wrapper function returning a class could be a potential remnant of npe1 due to lack or `magic_factory`.
    - Grzegorz: would be happy with only supporting menu contributions of magic factory and subclasses of QWidgets. Or purely functional which is later passed on to magicgui, but for now to have backwards compatability support wrapper until there is a complete solution.
    - Draga: There is a need for more possible contribution types. Now most people think a widget is always required to do something in napari.
    - Draga: stop advertising the wrapper. Just support backward compatibility for the plugins that already use it. At least for now.

- Grzegorz: v0.4.19 status 
    - Grzegorz: all PRs for 0.4.19 are either started or ready for review apart from the File -> Open issues with the File dialog and PyQt6
        - couple of options for fixing this: don't use native dialog, reimplement the missing functions, wait for qtpy to implement the functions...
        - need a decision on this
        - otherwise we can probably start moving to RC 1
    - Draga: if we need opinions on which way to go with the file dialog stuff, maybe post in core devs what the options are and we can go from there

- Grzegorz: telemetry NAP
    - Grzegorz: Draga mentioned in review that would be good to have a concrete list of what's being collected and perhaps a sample report, before NAP is accepted. Should I start working on that now, or wait?
    - Draga: I would say wait until it's merged in Draft form. I won't merge just based off of my review, but once we have a few more eyes on it we can merge and then you should feel free to look at the more detailed stuff
    - Grzegorz: Great. I think once we open the follow-up PR to get the NAP accepted, that will be a good time to reach out to broader community as well for feedback
    - Draga: agree, I think it's not specific enough at the moment and people will struggle to give meaningful feedback

## 2023-08-09

Present: Wouter, Draga, Genevieve, Lucy, Grzegorz, Juan

- Wouter: built-in docs?
    - Genevieve: another place to fall out of sync?
    - Wouter: CellProfiler has pop up help for most functions
    - Draga: user manual?
    - Genevieve: shouldn't solve this piece-meal, will make things even harder to find
    - Juan: app-model should help with this: all buttons have an action, all actions have their own documentation, accessible from Python or UI
    - Draga: people also have onboarding tutorials, which would be nice to have
    - Lucy: are we inventing Clippy?
        - Draga: yes but less cute and less useful :P
        - Could definitely be as cute! Nappy!
    - Grzegorz: possible to embed documentation in Qt
    - Grzegorz: docstrings won't be sufficient to document actions in the UI
- Genevieve: Author list on 0.4.18 Zenodo is kinda weird now https://zenodo.org/record/8115575/export/hx
    - Grzegorz: we can update this for 0.4.19 I think it happened because we sorted citation.cff
    - order needs to be decided for the paper anyway

## 2023-06-28

Present: Grzegorz, Juan

- [napari/npe2#301](https://github.com/napari/npe2/pull/301): codecov [indirect changes](https://app.codecov.io/gh/napari/npe2/pull/301/indirect-changes) tab showed that the logic was still a bit faulty: using a non-existent plugin results in a misleading error message, and no longer hits the past ValueError. We need to clean-up/flatten the logic flow (remove nesting) to make it clear that we raise the correct error message for each cause: plugin exists but contribution doesn't exist, plugin doesn't exist, plugin exists
- [#5997](https://github.com/napari/napari/pull/5997): 
- :point_up_2: your agenda items here!

## 2023-06-14

- Grzegorz: Test constraints https://github.com/napari/napari/pull/5904
    - trigger action not only on schedule/manual, but when constraints are editted
        - e.g. removal of plugin manager trigger the workflow
            - issue with circular imports (napari dependencies, upstream issue)
        - ready to review: when should it run and how?
    - 
- Wouter-Michiel Vierdag: fix positioning napari window https://github.com/napari/napari/pull/5915
    - small extra issues beyond the fix
        - if you move the window to external screen, the close and reopen napari will load on the primary screen
            - replicating is hard though
        - napari can also open in minimized fashion
            - Grzegorz: primary screen below secondary, so negative coordinates arn't handled
    - Juan: lets merge this so that the worst is solved, because the relative state will be preserved and user can interact
- Juan: brief demo of napari materials science (DefDAP) early plugin
    - SEM imaging of metal grain slipping
    - can click on labels and see the plots
        - issue with non-square/preserve axis
    - Wouter: is multi-selection possible? to get aggregated info?
- Wouter-Michiel: getting feedback regarding multichannel
    - multichannel grid view
        - same dim model, not true multicanvas
            - can't show labels layer on each viewbox, vispy doesn't allow multiple parents
    - synced zoom, drag via linked cameras
    - 3D not quite working, need to link 3d cameras both ways (vispy side)
    - Peter: overlay to add A B C ... to make figures


## 2023-05-17

- Patrick Cleeve, Lucile Naegele, & Rohit: discussion/demo of [OpenFIBSEM](https://www.biorxiv.org/content/10.1101/2022.11.01.514681v2), [Juno](https://arxiv.org/abs/2212.12540)
    - Lamella prep: preparing sample so it's thin enough to visualize via TEM
    - AutoLamella: automate the process and increase efficiency/throughput by reducing human interaction
        - Juan: what do you mean by tedious
    - Lots of clicking waiting and making rectangles progressively smaller--very monotonous
    - OpenFIMSEM: cross microscope control platform, modular. Thermo and Tescan now
        - abstract away the hardware API
        - Juan: so someone with a new microscope would just need to work on the bottom part, the API
        - what about Zeiss? working on it
    - Napari is most of the UI: shapes, labels, image zooming panning, scaling
    - Live demo!
    - Qt Designer to build the UI widgets
    - Shapes layer for the milliing
    - Also do training for moving needle/sample model: model assisted labeling (just tweak labels in napari)
        - uses Segment Anything (Meta)
        - Peter: do you think a wand tool or some other labeling tool would be useful?
            - Lucille: would be for cases where a whole model not needed, smaller scale
        - Genevieve: so you're using default weights and then using that?
            - Lucille: yes, we then train a more detailed bigger model. Once labeled we ahve a widget to train the model, which takes a while (feature detection)
        - Grzegorz: what format do you use for saving the models? Have you looked at bioimageio model sharing website and common format
            - Lucille PyTorch models default
        - Grzegorz: are you leaning towards ebedding napari or just napari plugin
            - Lucille: you do need a lot installed, but cannot distribute the manufacturer API, so installation and packaging is an issue.
        - Lorenzo: do you have access to the atlas of the whole grid?
            - Lucille: not yet, just the lamella with automove, but you can save positions
        - Genevieve: so this makes it easier to do the beam alignment?
            - Lucille: yea, we can abstract some of the hardware aspects
        - Lorenzo: is there something that prevents you from breaking stuff?
            - Lucille: yeah, API flags dangerous movement, plus we have checks to make some movements impossible. But it does depend on the geometry of the scope, so can't account for everything—need some user knowledge
        - Lorenzo: trusting your tool and automated movement with the expensive microscope could be :eyes: 
        - Peter: how did you settle on SAM for the UI? it has a lot of hype
            - Lucille: opportunity, it just worked
        - Juan: we've not seen issues from y'all, so suprised it's worked so well
            - Rohit & Lucille: we've been battling and sometimes not clear if Qt or napari issue
            - Lucille: close events are something, there is an open issue
            - Grzegorz: no event to check for a given window being closed, but you can check a Qt app closed. There are work-arounds. Multiple viewers are possible, so cleaning on close of viewer can be problematic. Also it's fragile for napari, could lead to crashes.
            - Juan: widgets are parented to a viewer, can that be used to clean up?
            - Grzegorz: with Qt designer, could be possible. Problem is with magicgui widgets
            - Lucille: want to send microscope command on close, nothing with napari UI
            - Rohit & Lucille: we've had some issues with that, Qt issues
            - Grzegorz: feel free to reach out using zulip, also weakref ptyhon way:
                - https://docs.python.org/3/library/weakref.html#weakref.finalize
            - Grzegorz: we need a FAQ in the docs for some of these things
            - Genevieve: and maybe also suggest Qt designer for complex stuff
            - Juan: blog post on that:           https://biapol.github.io/blog/johannes_mueller/qtdesigner_and_magicgui/Readme.html
            - Grzegorz: BTW I noticed a style incompatible with napari, this should be in the documentation, relating to widget parent
            - 
Genevive: I find some dock widgets get long, but no scroll bar
- Juan: yes we need this
- Lorenzo: we have this for the layer controls, can we lift?
- Grzegorz: it's not as easy becasue you need strong assumptions about the sizes and layout. simple solutions more often will finish with problems than help

Rohit: regarding Shapes, any plans to make boolean operations on shapes? like to make a donut shape. Right now need to add that as a image
- Juan: we have issues with polygons with holes right now, but we don't have an issue on booleans for shapes
- Lorenzo: we have some issues with vertexes too selecting/deleting, so please open an issue
- Juan: many features could be implemented as a plugin

- :point_up_2: your agenda item here!


## 2023-05-03
- WM: [#5555](https://github.com/napari/napari/pull/5555) Napari lasso polygon: exposing epsilon parameter of rdp algorithm to user

### 2023-04-19
- Grzegorz: Enforce better review/maintenance practices: [label action](https://github.com/napari/napari/pull/5733) and [milestone check](https://github.com/marketplace/pr-milestone-check) 
    - merged
- Grzegorz: Pinning test dependencies: https://github.com/napari/napari/pull/5715
    - need to proper place constrains 
    - add script to report updates in PR comment 

Put Your item here

- Grzegorz: Compiled backend on example of PartSeg https://github.com/4DNucleome/PartSegCore-compiled-backend/pull/3
    - not discussed

### 2023-04-05

- Draga: re: [npe2 case sensitivity PR](https://github.com/napari/npe2/pull/275#discussion_r1158021861)we should make things case insensitive and update docs. The original path should be passed to the plugin, though, just the matching should be insensitive.
- Juan: coloring labels in shaders [#3308](https://github.com/napari/napari/pull/3308)
    - Juan: massive speedup, only catch is can't display differnet colors for adjecent high value labels due to fp precision (e.g. 2^24 and 2^24+1)
- Grzegorz: psygnal PR merged (https://github.com/pyapp-kit/psygnal/pull/200) towards cross-thread events
    - Juan: should napari move more to psygnal events?
    - Grzegorz: would be a speedup, but a lot of work
    - Peter: maybe a roadmap/contract thing?
- Peter: shortcuts PR (https://github.com/napari/napari/pull/5679), any feedback?
    - 👍 
- :point_up_2: add your agenda items here

### 2023-03-22
- Wouter: Qt/VisPy decouple PR could be ready for merge, one more round of feedback
    - docstrings/documentation of private attributes
    - JNI: import to document private stuff too
    - Grzegorz and Juan agree: we should find a setting to not render private attributes in the docs
- Peter: layer visibility contextual menu [PR #5574](https://github.com/napari/napari/pull/5574)
    - gist of discussion: two menus OK, split show/hide if seperate menu
    - also open issue on linked layers toggle behavior (not related to this PR)
- Wouter: Polygon lasso [PR #5555](https://github.com/napari/napari/pull/5555)
    - discussion: extra button? or change behavior of existing polygon
        - Peter: extra button preferred because of change of default polygon behavior
        - Grzegorz: is 5 px good for every image? is it screen or image px?
        - JNI: should be screen?
        - Grzegorz: may need to use both, so you don't have too dense points
        - JNI: later PR? to use canvas space in the future? Current architecture may be impossible
        - JNI: the 5 should be documented!
- Peter: Zoom button gives KeyErorr and space keybind don't work
    - and switching to zoom exits shapes editing
    - Grzegorz: zoom is a mode, so it exits Edit mode, should maybe not be a layer mode?
    - Peter: would be nice to be able to always zoom, while doing other things

#### Action items

- Juan: comment that private attributes in docs should be a sphinx config (and we can put resources into that if needed)
    

### 2023-03-08

- Wouter: Qt/VisPy decouple PR
    - Grzegorz: would be good to be able to configure the VisPy Canvas class used by the viewer. e.g. pass argument `vispy_canvas_class=VispyCanvas`, then instantiate with `vispy_canvas_class()`. Ex: [`actual_factory` in `make_napari_viewer` fixture](https://github.com/napari/napari/blob/ff1e56ef8e44fa0636d6295e5a001e443624b5e7/napari/utils/_testsupport.py#L221).
- Peter: layer visibility feedback on [#5574](https://github.com/napari/napari/pull/5574)
    - plan: remove scope creep (alt click behavior) to new PR
    - for the alt click, check interaction with selected layers
    - for the contextual menu, Juan would like show-only selected
        - maybe this should be an alt-option in the menu?
- Grzegorz: any objections about: [#5198](https://github.com/napari/napari/pull/5198)
- :point_up_2: add your agenda items here
- Juan: brief updates on [napari/napari-animation#154](https://github.com/napari/napari-animation/issues/154), [ome-zarr-py](https://github.com/ome/ome-zarr-py/pull/253), [mysquishy](https://github.com/jni/mysquishy)

### 2023-02-22

- Peter: can we look at [#5574](https://github.com/napari/napari/pull/5574):
    - change "toggle visibily" to "make visible" or "make invisible" depending on state of layer under cursor. Remaining selected layers toggle "in sync" with the layer under the cursor
    - Grzegorz: too many grayed out options make menu useless. We should gray out options that are available for this layer type but somehow not currently. But not options that don't exist for this layer type at all.
        - Peter: if a user right-clicked on a points layer and saw options, I think they could expect to see something for image, etc. so this wouldn't be so bad.
- feature discoverability: can we have tips and hints at startup?
    - Peter: like video games even a simple tip "try right-click on a layer", "try right click on grid button" can help
- Grzegorz: can we simplify app_model contributions? eg. see [#5574](https://github.com/napari/napari/pull/5574/files).
    - Peter/Juan: would be nice to see mockup before/after for that PR with new proposal.
    - To make it more interesting, would be nice to add a keyboard shortcut to the same PR, to make the before/after mockup "complete". From Grzegorz: [Adding keybindings](https://github.com/napari/napari/blob/e669a80636da1b63f068a1a473582e1f64a67ff4/napari/utils/shortcuts.py#L86).
- Keybindings are currently broken because of incomplete transition to app model, and this is blocking release:
    - command/meta swapped on macos [#5332](https://github.com/napari/napari/issues/5332)
        - Peter's hypothesis: app model and Qt both switch Cmd/Ctrl for Mac automatically, and the number of switches is now incorrect.
    - changed keybinds not saved https://github.com/napari/napari/issues/5579
- Grzegorz: Another app model issue: when changing shortcuts, app needs a restart or the old shortcut will still work.
- :point_up_2: add your agenda items here

#### action items

- #5210 - could cause memory leaks

### 2023-01-25

- Herearii - manini plugin 
    - Herearii: how to advertise or share a new plugin?
    - Grzegorz and Peter suggested making a post on image.sc, after making sure the README and other content is up-to-date. Can also post on twitter, just make sure @napari_imaging
    - Herearii: what about the CZI plugin grants?
    - Lorenzo and Peter: it was in the spring, so monitor the CZI funding webpage. Also monitor the zulip
    - Herearii: poster promoting plugins with napari icon: is it free to include the icon?
    - Grzegorz and Lorenzo: yes, it's fine. We have no official collaborations.
    - Peter: you can also cite the repo
    - Grzegorz: consider putting your plugin on conda (conda-forge) to make binaries more broadly available, especially if you have specific dependencies
- Grzegorz shared an issue to PartSeg: https://github.com/4DNucleome/PartSeg/issues/891
    - theme change resulted in blending change, user didn't know how to change back
    - issue persisted even with new env, because settings are saved/stored not silod
    - the underlying issue is solved in `main` napari, but not released
- Prince: favorite plugins everyone?
    - Discussed a few

### 2023-01-11

- Prince: intro
- Alister: poor man's octree basis demo: color points based on distance to camera centerline. Code here: https://github.com/alisterburt/napari/blob/poor-mans-octree/examples/are_the_points_in_view.py ([permalink](https://github.com/alisterburt/napari/blob/44506f37cf3a7e113c05216bbe09a2f170952ed2/examples/are_the_points_in_view.py))
- [#4436](https://github.com/napari/napari/pull/4436):
    - changing the vendored code is too heavy — can we get away with not making those changes
    - magicgui private attributes shouldn't be used
- Peter: What's the deal with vendoring?
    - prevent some other package updating our dependency under our noses (because pip does not take global constraints into account, only constraints of current package being installed)
    - use small bit of code from big library (e.g. matplotlib colormaps) or small library that may not be well supported, so it is risky to depend on it.
- Grzegorz: [#5407](https://github.com/napari/napari/pull/5407)

#### Action items:

- Grzegorz: summarize issues with #4436, preferably with a way forward 😅
- Juan: comment on #5407; ask for clarification from OP about motivation; clarify our minimum requirements to accept PRs (public API)

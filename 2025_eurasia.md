

## 2025-04-30

### Attendees
- Juan, Draga, Grzegorz

### Topics
- napari-plugin-manager 0.1.6 is out yay!
    - shouldn't need any constraints/requirements updates
    - do need to get the conda-forge version updated
    - https://github.com/conda-forge/napari-plugin-manager-feedstock/pull/22
- packaging/#235
    - removing `defaults` channel in bundle
    - tensorflow conda version is v old, should we not provide tensorflow in bundle?
    - GB: no, because Windows conda-forge doesn't have it, and the `defaults` channel is paid for
    - can provide docs to have folks install separately
    - PR merged
- Release stuff
    - need npm on conda forge
        - merged PR etc.
    - need a bunch of docs updates, and the release notes, and then tag release

## 2025-04-16

### Attendees

- Wouter-Michiel, Grzegorz, Juan

### Topics

- Add your items here! :point_down: 
- WM: [#7779](https://github.com/napari/napari/pull/7779)
    - Discussed issue with axis reordering when exporting figure
    - WM confirmed issue with axis reordering when exporting figure also in 
    2D on main, taking screenshot is unaffected both on main and in PR.
- Juan/Grzegorz: polygons with holes with any backend [#6654](https://github.com/napari/napari/pull/6654)
- Juan/Grzegorz: State of [#7747](https://github.com/napari/napari/pull/7747) swappable triangulation backends

### Action Items
    - Juan: decide whether to land #7779 in 0.6.0 given that it is no regression from main.
    - WM: revive PR for synced multichannel view. Schedule paired coding sessions with Lorenzo.


## 2025-04-02

### Attendees

- Grzegorz, Wouter-Michiel, Juan, Draga

### Topics

- Tim (will be sleeping) - Please briefly request opinions on [#7765](https://github.com/napari/napari/pull/7765). :grin: and the handedness icon in [#7770](https://github.com/napari/napari/pull/7770), but I'll bring up Thursday/Friday if the same people are there anyways.
  - People love it (#7765)
  - People also love (#7770)
      - WM suggested potentially using the anatomical names e.g. anterior/posterior or the acronyms that indicate each axis
      - Draga & Juan thought that would be too discipline specific, while the hand is more generic
      - Otherwise, definitely need documentation for what these are (already part of the 0.6.0 milestone)
      - we're pretty happy with the implementation
- Draga - plugin manager stuff
    - https://github.com/napari/napari-plugin-manager/pull/131
        - no need to update tests, Juan will test locally on mac
    - https://github.com/napari/napari-plugin-manager/pull/143
        - looks like we might not even need the warning anyway because menus are updating
        - change text to "you may need to restart" and not mention npe2
        - update implementation to store a "original_list" on show and then just diff it on hide to decide when to show warning
        - eventually add code to check environment changes and only warn if there's been a change in some core packages
- Grzegorz [#7739](https://github.com/napari/napari/pull/7739) - unblock polygon with holes
    - improves benchmarking for the polygons backends
    - checks four options for speeding up triangulation: pure python, numba, triangle and bermuda
        - pure python is just pure python
        - triangle is triangle + numba
        - bermuda is bermuda + numba
    - eventually should be exposed in settings
    - could use module level `__getattr__`, but Grzegorz concerned about performance of using `__getattr__` in a hot-loop
    - Juan will review
- WM - request opinion steering council and core devs
    - Mikaela (sp?) has enquired about more collaboration between scverse and napari
    - getting some questions about napari
    - Grzegorz also currently working on spatial stuff
    - can there be an "official partnership" between the two projects to facilitate easier communication, potentially merged hackathons, etc. 
        - e.g. a "napari" portion of the scverse hackathon
    - probably a good question for Tracy/Carol
    - a partnership that involves Genentech could be very advantageous
    - dedicated code cafes for implementing certain features would also be an option

### Action Items

- Draga will update napari-plugin-manager/#143 warning as above
- Juan will test napari-plugin-manager/#131 locally
- Juan will review napari/#7739

## 2025-03-19

### Attendees

- Wouter-Michiel, Juan, Grzegorz, Draga

### Topics
- 0.6.0 status
- Polygons with holes #6654
    - Grzegorz latest comment suggests not reverting latest commit, because pure Python implementation is faster
    - Juan agrees after discussion
- new vispy release needed with https://github.com/vispy/vispy/pull/2647
    - Grzegorz pinged Lorenzo on issue
    - there's also a vispy issue with numpy 2 where benchmark crashes
        - we think it's because of set membership checking with mixed int/numpy int 64 types
        - Grzegorz' plan is to check if benchmark is crashing due to this issue and then report back
- #7725 and #7723 could be quick
    - https://github.com/napari/napari/pull/7725
        - Windows can't access `echo` so yes, makes total sense these need to be removed
        - they were only for debugging purposes anyway so not a big deal to remove them
        - Juan merged
    - https://github.com/napari/napari/pull/7723
        - we can just close this one after merging #7225, as it's no longer relevant and anyway it crashes CI
- Juan: View menu sections: https://github.com/napari/napari/issues/7720
    - Toggle full screen needs a `when` on it, because mac adds its own `Enter full screen` option (that's why there's two of them)
    - otherwise agree that we need to rename the groups as they're no longer meaningful
- Juan: https://github.com/napari/napari/pull/7721 View menu
    - probably better to just not have the shortcut there for now, because we can't edit it
    - Grezgorz will start looking into reintegrating our keybindings and keeping the work on app-model migration going
    - we should also be able to re-build the menus when registering a new plugin rather than having to restart napari just to get a plugin showing up in the menu
- w-m: toggle scale bar settings
    - would like some more control over the scale bar color etc. that is persistent
    - should go in settings for now, because we get all the synchronising/updating machinery
    - should also link from View menu and have the option open settings to the right tab
    - ideally we could also have the same widget triggered 'modally' so that the setting is **not** persistent
    - Wouter-Michiel will work on the PR to add the widget to settings initially
- w-m:feedback ebi industry event
    - quite a few folks filled out the form
    - many people working with multiplex images wanting multiple viewers
    - people interested in use cases deploying in a cloud setting
        - we could probably get some initial documentation going for even the janky methods
    - potential for collaboration with VITESSCE viewer on bermuda goals and web viewing work
        - bermuda especially we should consider what other parts of the codebase could benefit from compiled backend
        - GB thinks almost everything that's under numba could benefit from compiled backend
    - definitely want to be aligning on the serialization specs for viewer configuration for 2D and 3D views
    
### Action items

- Juan: update #7721 to use ViewerModel instead of Viewer
- Juan: Make issue about adding `when` to `Toggle full screen` option in `View` menu: https://github.com/napari/napari/blob/main/napari/_qt/_qapp_model/qactions/_view.py#L85
- WM: make docs issue about documenting our deprecation utilities

## 2025-03-05

### Attendees
- Juan
- Grzegorz

### Topics
- 0.6.0 status
    - change status default print for coordinates to use smart float formatting
    - allow copying of status bar with keyboard shortcut
    - blog post about debugging compiled backends=
    - bug about edge meshes introduced in the unified dispatch PR

### Action items

## 2025-02-19

### Attendees 
<!-- Thanks for joining. Please add your name and github handle here -->

- Wouter-Michiel
- Juan
- Grzegorz
- Baptiste Roelens
- Draga

### Topics
<!-- Please add items for discussion here. Thanks! -->
- Welcome newcomers to the meeting.
- @willingcn: napari 0.6.0 would it be possible to message rough dates to the zulip releases channel for visibility. Thanks.
- Big stuff still needed for 0.6.0
    - axis swapping interface
    - polygons with holes in the absence of acceleration
    - inheritance of scale etc metadata in functional plugins
    - warning on loading of shimmed plugins
- Draga: warning on loading of shimmed plugins
    - PR is almost ready, and will warn users on start-up about any npe1 plugins that are being loaded in shimmed form. Users can turn off the shimming behaviour for now.
    - Known limitations to the shimming behaviour are plugins that do things on import/on startup and plugins with mulit-layer writers
    - Draga is working on a document that we can refer people to, and we will link it in the dialog and possibly also in preferences
- All: 0.6.0 triage
- WM: @jni would be good to have a response here: https://github.com/napari/napari/issues/7620
    - Juan will respond that we're still somewhat working on a paper, and thank the author for the provided script

### Action items

- ~~Juan: message Zulip about 0.6.0 timeline~~
- Juan: respond on #7620
- ~~Juan: follow up to https://github.com/napari/napari/pull/7487 in docs updating the makefile~~
- ~~Juan: review+merge Numba warmup~~
- ~~Grzegorz: review 7622~~

## 2025-02-05

-
- all: napari 0.6.0 plans
  - viewer handedness/orientation
  - pydantic 2+

- [#7580](https://github.com/napari/napari/pull/7580) - indicator for right click menu 

## 2025-01-22

- Wouter-Michiel: overlays 3.0
  - mixins call super but themselves don't have super
  - Lorenzo: example in examples/dev on how to create an overlay
- Juan: constraints update after napari-sphinx-theme 0.6.0 release
- Juan: release 0.5.6
    - will happen after 7538 and after meeting
- Grzegorz: 7541 https://github.com/napari/napari/pull/7541
    - threading of compilation might confuse users because it does not have any visual indication that it is working.
- Grzegorz: crazy bug in shapes transformations: when a shape reaches size 0 along an axis, it becomes broken
  


### Actions

- WM will create a w2m for overlays with jni and brisvag
- ~~JNI make issue with 0.6.0 milestone to bump deprecations~~
- Grzegorz: make issue about transforming shapes

## 2025-01-08

- Juan: camera flip in [#7488](https://github.com/napari/napari/pull/7488)
    - flipping makes the shoe model correct, but the lighting direction is wrong
    - Lorenzo made a fix so that the lighting is always next to the camera in #5893
    - now in #7488 the cross product has changed polarity, so first just tried to flip it again by multiplying by -1 on z, but that doesn't work
    - also tried subtracting the cross product instead of flipping it, and that "kinda" works
        - that's closest to main, but too bright in some angles
        - brightness is actually also occurring on main though, so this is probably the correct fix
    - there is also a follow-up PR that adds the option to flip individual axes
        - we need options that work for e.g. geodata and anatomical data
        - to be as generalizable as possible, we should just indicate which point of an array to use as origin, and the direction the axes move from origin
        - for each displayed axis we could have the number and which way it's pointing (up/down or left/right)
        - need docs that link these different orientations to what users are most used to
- Juan: YAZ3F (yet another zarr 3 fix) in [#7497](https://github.com/napari/napari/pull/7497)
    - approved and merged
- Layer controls refactor [#7355](https://github.com/napari/napari/pull/7355)
    - Grzegorz left review today but may not have been exhaustive
    - some attributes are camel-case and some are snake-case
        - we should use snake-case for all of our code, so that camel-case is reserved for qt code
    - docstrings are documenting sub-attributes, and we should avoid that because people will forget to update the docstring
    - some attributes are still added dynamically, which means we won't get proper IDE linting and it may not be obvious where attributes are coming from
        - rather than passing widget constructor to `_add_widget_controls`, we should declare the attribute separately, name it ourselves, and then pass it to `add_widget_controls` by name. This will give us control over the attribute names and make sure we don't run into trouble if we reuse a class
    - Grzegorz will follow up with the rest of it
- Triangulation speedups #7346 and bermuda work
    - we need someone to review more closely
    - we will release 0.5.6 soon, perhaps we should merge this all with optional PartSeg core dependency, so that spatialdata folks can start using it and get feedback
    - Juan will have a look tomorrow and see if we can get it in for 0.5.6
- [w-m] https://github.com/napari/napari/pull/7209
    - For the screenshots we would like to test the actual values but it seems like events are not processed inbetween screenshots.
    - Previously discussed use of `process_events`, which did not work for testing the actual values. Other suggestions?
        - Grzegorz will have a look at it
- :point_up: Add agenda items here! :point_up_2: 


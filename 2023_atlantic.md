## 2023-12-20

- Kandarp: napari survey "types of images" question (~15 minutes)

## 2023-12-6

- Linking labels to image data, w-m write up different ways of user interaction for this.

## 2023-11-22
    
* Grzegorz [napari/napari #6467](https://github.com/napari/napari/pull/6467)
* Isabela: [layer groups issue update](https://github.com/napari/napari/issues/6345#issuecomment-1822232131). 
    - Spreadsheet with comparison of software similar to napari is available with respect to how the different softwares deal with layergrouping. Available in the issue.
    - [kyle] Perhaps it would be good to also check out blender to learn more lessons from software for 3D.

## 2023-11-08

* Grzegorz [#6411](https://github.com/napari/napari/pull/6411) Fix lagging 3d view for big data

## 2023-10-25

- Grzegorz:  Simplify test dependant packages against napari [#6336](https://github.com/napari/napari/pull/6336)
    - Potential use cases: in npe2, in-n-out, the installation manager etc. -> to see if upgrading these packages would break either main napari or an existing napari release version (github action workflow). The napari tests are run after installing the new package version (e.g. [npe2](https://github.com/napari/npe2/pull/322/files)).
    - Possibility to have a cron job (or similar) that could run on a regular basis to check changes against VisPy, dask, zarr etc. and then list the packages that should be included in the cron job.
        - Would these be managed if a cron job was created? Maybe a zulip bot.
        - This workflow could be used to make such a cron job.
    - Could the test cases be reduced to a subset of the whole test suite to be a bit lighter on the CI?
        - Could use the `env:PYTEST_PATH` to achieve this (already there).
    - The workflow currently manages the napari versions that are checked.
- Grzegorz: v0.4.19 release status
    - Highlights - https://github.com/napari/napari-release-tools/pull/3
    - A few outstanding PRs with [0.4.19 milestone](https://github.com/napari/napari/pulls?q=is%3Aopen+is%3Apr+milestone%3A0.4.19) if time to review any.
- Lorenzo: use [ruff-format instead of black](https://github.com/napari/napari/pull/6383)
    - The changes between the two are primarily known deviations between ruff and black.
    - Maybe make this after 4.19.

## 2023-10-11

- Future hackathons!
    - Suggestion: run this by the steering council to organise how future hackathons might want to go for napari. Future hackathons could be in EU, NA, and many places.
    - In future hackathons there could be something included to help with generating ideas in a casual way - e.g., a hike. Related to this, desired length of hackathon.
    - University affiliated accomodation could be a cheap way to allow people to stay over for the hackathon.
    - Funding could be potentially obtained for any of these hackathons from general bodies following this.
    - Ask steering council
- Isabela: I'm starting to look into [layer groups](https://github.com/napari/napari/issues/5950) from an exploratory design perspective.
    - Starting to gather some reference material on how hierarchical concepts/grouping behaviours are designed. These could help going forward for discussions and what we'd like to have with layer groups.
    - Is there an issue on GitHub that gathers and consolidates the various discussions around this?
        - Let's try use the layer tracking issue to link out to the discussions and other issues that will be created.
        - The tracking issue can be expanded to capture the new issues.
    - The priority might be to make it better for users to do single nesting vs all the N dimensional nesting. This is because it might be the most common use case.
    - There may be some use cases where layer groups are kind of standing in for a solution to having multi-canvas setup or [NAP-3](https://napari.org/dev/naps/3-spaces.html). This would enable some convenience, as opposed to enabling solving something that users are blocked from doing.
    - We are in a space where there may not be any pre-existing solutions that really get to the core of this problem.
    - As an example, you could represent position in space via a points layer, and orientation via a vectors layer. These two thing are linked, so whenever you want to hide one, you'd want to hide both for e.g. . Or updating a property on one could also on the other.
    - The ability to register 2 layers together could be useful. Such as stitching. Overall, this is the bigger feature of being able to do things with multiple layers. Is it the job of a layer group to enable this multi-layer operation?
        - Not the job of the layer group, but you can use layer groups to do it. In other words, you could do it by selection or by layer groups. Layer groups might just be an easy way for people to do this.
        - This could feed into better anotation ability in napari.
        - Similarly the idea of merging layers has come up often. Layer groups could be used to enable this in the future.
    - Going forward, the example of tiled transform could be a good one for thinking about layer grouping.

## 2023-09-27

- GB #6211 merged: how to use it
- Sebastian [linked issue](https://github.com/napari/napari/issues/6148):
    - Using a single layer for many many points, but data shader allows as a backend to render 10million points quite smoothly.
    - Example 1: Millions of points
    - https://datashader.org/
    - Perhaps similar to generating a multi-scale image because it creates a histogram style rendering
    - Possibility to have backend like a pyramidal zarr
    - Could there be a custom layer to have different backends
    - Example 2: Many thousands of polygons (e.g. >100,0000)
    - datashader is faster for both of this, and then the image can be passed into napari
- WM: Updates to the scale bar in progress to improve the appearance of the scale bar - especially for screenshots
    - Works well for 2D at the moment (PR 1)
    - Would need updates for 3D though (PR 2)
    - Screenshots of ROIs (PR 3)

## 2023-09-13

- KH: Thoughts on @jni's layer grouping [idea](https://github.com/napari/napari/issues/5950)
    - What could we do here in a design space to pull out the ideas of how a non-graph/tree heirarchy structure could look from a design perpective to help drive the ideas forward here.
    - Lorenzo has been thinking about layer-data separation using request/response machinery. This, because a solution where canvases share the same layerlist, camera, dims, could otherwise be challenging.
    - Possibility to have underlying ground truth data that is then viewed/interacted with differently by layers. E.g. different layers could have different colormaps to view the same underlying data.
    - Where would the border be drawn between what is part of the view and what is part of data itself. E.g. is scale a core part of the data.
        - Perhaps just creating a big table that splits attributes into data and view to show which parts belong to data/view
    - Metadata from NGFF might get you some baseline features which could then be overwritten by the view.
    - Having a layer/data that has properties that could then be overwritten by the view.
    - Maybe two independent tasks:
        - 1. introduce a tree like structure. Can layer groups be part of multiple layer groups?
        - (Perhaps VisPy loads data once and then creates multiple viewer nodes)
    - Example vispy: https://vispy.org/gallery/scene/one_scene_four_cams.html
    - Next step for all of this might be a NAP

## 2023-08-30
* WM: Contrast limits
    * Currently it seems that the contrast limits can be set twice. Is this behaviour intended? It happens both in `_ImageBase.__init__()` and in `_update_slice_response` within the same class which is triggered by the `self.refresh` in `__init__`. The way of fixing the contrast limits issue depends on the answer to this question. In particular, do we want the contrast limits to be based on the slice?
    * Related PR to this from Grzegorz [here](https://github.com/napari/napari/pull/6190/files#diff-584ea8a3ec32725bbee7f799fdc19880710877d2ecfde027bea4951fe36e2a19R485-R526)
    * New layer_utils function to get chunk size based on the data type.
        * Should be added to have `xarray` support
    * No sampling heuristic here that will suffice for all cases, metadata would be best. However, to support contrast sampling the centre and the corners might be good.
    * For now, a good solution might be to change 64x64 sampling to 1k x 1k.

* GB: support of system napari: https://github.com/napari/napari/issues/6196
    * Difficult to detect in a universal way that napari would be on the system Python. So the package manager could set a default false variable to true instead and then that would be used.
    * Possibility for this to be a preference in napari - but might be better to put it in a separate file that is easily patchable.
* Isabela: layer controls design update: details on the tentatively chosen direction! [Most recent designs](https://github.com/napari/napari/issues/5358#issuecomment-1699362809) for feedback.
    * Related PR about icon shifting [here](https://github.com/napari/napari/pull/5021).
    * You can swap between the lower buttons in napari by clicking one of them, and then navigate through them via the keyboard (e.g., instead of arrow keys moving slices -- they move between buttons).
    * Related merged PR about button states [here](https://github.com/napari/napari/pull/5262).

## 2023-08-16

* Grzegorz: Drop python 3.8
    * Just a note, python 3.11 is already supported by napari (since 0.4.18).
    * Wouter: Consideration here for mypy due to a lot of changes for version 3.9
    * Some plugins might not be ready to move to the new version, but is that because they are not maintained or have specific requirements (seems there are only 3 of these).
        * Wouter: suggestion to get in touch with the people who are in this case and figure out why.
    * Juan: annotated type hints will be nice going forward to use, but that probably won't be in 0.5.0. Perhaps just drop 3.8 support as soon as there is a PR that needs to go past it.
    * Grzegorz: new typing in 3.9 is much better (e.g. | instead of Union, and you can use `list` instead of `List`).
    * Consensus: 0.5.0 will drop 3.8 support.
    * Possibility for hackathon of a tools menu.
* Sean: Double check the relationship of plugins on PyPI vs conda-forge
    * Currently, the napari plugin cookiecutter gives you a PyPI based released system out of the box
    * At the moment, the bundled version of napari only supports installing plugins from conda-forge
    * The plugin installer is being updated to support conda-forge and PyPI (perhaps for 0.5.0?) so that you can:
        * Install PyPI only plugins/plugin dependencies from the bundled napari app
        * Install conda-forge plugins via conda where napari is installed (non-bundled version) in a conda environment.
    * (Context): workshop in development, and request to discuss where plugins should "live". So I wanted to talk about the current status of plugins being on PyPI and conda-forge. Here, I'm trying to see if I have this all correct.
    * Encouragement - make all plugins on conda-forge, however to upload to conda-forge requires forking the feedstock. This could be documented for the process.
        * What is the benefit? (Joel to Juan). Installing things in PyPI can be awkward due to the resolver. Conda-forge protects you from most of these problems.
        * Also helps with the problem of PyPI packages being released with no wheel builds that have C extensions.
    * Related links: https://github.com/conda-forge/staged-recipes/ and https://conda-forge.org/#add_recipe.
    * Lack of documentation around the technicalities here.
    * https://github.com/conda/grayskull works sometimes for the automatic conversion.
* Isabela: layer controls design update: spacing! [Most recent designs](https://github.com/napari/napari/issues/5358#issuecomment-1680800271).
    * Changing how the titles look in the widgets (in general in napari) could be a good thing to do, probably separate to this change though.
    * Right aligning the text in the layer controls could be nice.
    * Truncating in the middle of the text for the layer list could also be a nice overall change (link from Andy https://doc.qt.io/qt-6/qt.html#TextElideMode-enum).
    * Ashley: If vertical space is a huge problem we could enable dock widget nesting so layer list could go to the left of the layer controls.

## 2023-08-02

* Isabela: layer controls design update! [Most recent designs](https://github.com/napari/napari/issues/5358#issuecomment-1642465035).
    * Proposal for how to have layer controls in the viewer, walkthrough in github issue.
    * Option 1 - focus on persistence of buttons across layers (each layer has the same sections). Features are grouped into functional groups.
    * Option 2 - buttons are not persistent across layers. Actions are split into functional groups. Options 2 and 3 are the same in terms of the grouping of actions in terms of functional vs base and layer specific.
    * Option 3 - actions are split into base and then layer specific (e.g. base options for all layers would be opacity and blending).
* Jaime: 
  * `menuinst` [proposal](https://github.com/conda-incubator/ceps/pull/8) (cross-platform menu items / desktop shortcuts for conda) should pass tomorrow, which means we can upstream everything we have done for the bundled conda-based installer.
  * Also, thanks to Gonzalo we now have a [project](https://github.com/orgs/napari/projects/25/views/1) to track napari plugins on conda-forge if you are curious
* Chris: lock dimensions while rolling -> current standing ([#5986](https://github.com/napari/napari/pull/5986))
    * Ability to lock individual dimensions via right clicking on the dims rolling button. You can't move dims while they are locked currently.
    * Looking for feedback. At the moment the `QtDimsSorter` is instantiated and events hooked up every time the button is right clicked. Possible implementation - use the until signal. This works, but is not really the intended use case of that connection parameter. `QtDimsSorter` is dependant on the surrounding popup widget - this breaks some tests at the moment.
    * Question 1 - do we need the security around `move_indices` in `qt_dims_sorter`? It seems this security is never needed because the popup always closes.
        * Different solution, one could add callbacks to the `QtDimsSorter` to not remove the security feature.
        * Andy idea: This is a `QWidget`, there are events associated with signals that are deleted. You could maybe figure out when it is destroyed to disconnect the callback. Chris: in this context, if there remains a reference to the `QtDimsSorter` instance, it won't get destroyed. So he can't couple it. Instead it is coupled to the destroy from the container popup menu.
        * Overall, it seems maybe this security goes.
    * Question 2 - the parent of the `QtDimsSorter` could become mandatory, or another parameter that is mandatory needs to be introduced, which to do? Follow the principles of QT to keep parent optional? Lorenzo: maybe it could mocked in tests since that is the only place that the parent is really ever `None` - will likely go mandatory parent.
    * Question 3 - anyone with experience in design to make the color for the lock change?
        * Isabela: reference PR to help [here](https://github.com/napari/napari/issues/3732).
        * Andy: maybe use font-awesome icons because they have license in the svg files.
        * Peter: the pan icon is already from font awesome.
        * Lorenzo: changing color on click would be really nice (for locked/unlocked).
        * Chris: the popup being bigger would be nice, but it seems there is a bug in QT that prevents this.
        * Grzegorz: the styling simplest thing is to remove everything associated with shape (Andy tried this though). [Style defined here](https://github.com/napari/napari/blob/21bc62443ae184a8d1576eefb1150c300c15e3ac/napari/resources/_icons.py#L31).
    * (Raising feature visibility) Peter: there are initial docs, maybe could use twitter/mastodon etc to provide visibility, Lorenzo maybe welcome screen, Grzegorz, maybe option that your cursor changes over options that are right clickable. Jaime: Maybe adding a dropdown icon next to it helps signals it? I think some folks call it “Split button”. Overall there could be scope here in UI to add visibility. Or a small tick mark in a corner like Photoshop tools.
* Andy: [cryoET data portal plugin](https://github.com/chanzuckerberg/napari-cryoet-data-portal)
    * Collating data together from multiple labs to create a public domain portal with accessible large data.
    * Andy has created a napari plugin to browse and visualise this data. Data is on S3, graphQL used to list the data and browse it. The plugin is built in QT.
        * Can view runs, metdata and some point annotations in the tomograms.
        * Maybe the vectors layer could be used to show the orientation of the ribosomes, for example.
        * Napari-omero is another tool that is used a similar browsing fashion. Maybe there could be a tree type structure to look at this.
        * Peter: I really like the progress bar. A lot of plugins do things that can take time (download data, process whatever), but there is minimal if any indicator something is happening. Andy has built on topic of the thread worker to have a progress bar. Maybe in magicgui there could be something like this.
        * Grzegorz, maybe these kind of plugins could behave a little differently to other plugins so that they could be allowed some more space, like a data viewer panel. Lorenzo, the first version of what he worked on had this idea, but having layer groups in a tree that mimics the file tree might be more desirable. Maybe the layer group idea does not scale to 1000s of files though.

## 2023-07-05

- Short meeting!

## 2023-06-21
* Kushal Kolar, Caitlin Lewis, Eric Thomson: fastplotlib: https://github.com/kushalkolar/fastplotlib
    * We briefly introduced `fastplotlib` last summer, a next-gen visualization library that targets Vulkan (or DX or Metal) using which are the successors to OpenGL. `fastplotlib` is built upon the `pygfx` rendering engine
    * We've made significant progress with `fastplotlib` in the past year. Last year there was some interest in potentially using it as a back-end for napari. We're open to reconsidering this possibility!
    * We're also generally open to any other questions or interest people may have in `fastplotlib`
        * Kyle: whats the relation to vispy2?
        * Kushal: it seems not to be happening, so took it into own hands, there is also datoviz which is on Vulkan
        * Ashley: see also https://github.com/vispy/GSP
        * Grzegorz: consider talking with Lorenzo, vispy contributor/maintainer
    * motivation: was trying napari plugin (year ago), but it wasn't fast enough and some api limitation
    * can be an app (qt) or notebook
    * "expressive" every level of fancy indexing, to change, for example color
    * Andy: how do you see integration/interaction with napari? what about your existing users?
        * Kushal: majority of users are in calcium imaging, we do heatmaps and contours (5000+) and also visualize behavior. want to look at everything at once
            * some people do want realtime/live reading/plotting
        * Kushal: at fastplotlib level we don't hide pygfx, e.g. events system, similar api. so integration could happen at both levels, maybe more sense at pygfx level, because it's the rendering engine and then abstractions from fastplotlib
        * Kushal: no volume rendering, by pygfx has it. could probably add it to fastplotlib, same for meshes/surfaces
        * Kushal: we're presenting at scipy!
    * Eric: caiman developer, uses fastplotlib -- large (up to Tb) files, users like jupyter, and wanted non-blocking viewing, would like to build out more developers and users -- scientists, jupyter vs. (py)qt, etc. How hard would it be to connect to napari world? Trying to increase the bus factor on fastplotlib.
        * Grzegorz: whole gui is in Qt, but we try to keep separation. we have plan for web interface, but not implemented. for widgets simplest is to use magicgui, which also provides jupyter backend, so same code can work. So simplest: decorator, then container with widgets, then for most complex qtpy (qt directly)
            * napari-matplotlib is a plugin to add plots next to data, so fastplotlib could act in same role -- napari to view data, fastplotlib for more responsive plotting 
                * may be possible to add fastplotlib widget to magicgui
        * Kushal: can you use your render engine to make the UI? like imgui? same everywhere
        * Kyle: at the moment it seems beyond the roadmap
        * Grzegorz: need to write your own framework, we'd need compatibility accross all platforms


* Christopher Nauroth-Kreß: Locking dimensions while rolling (dimensions)
    * e.g. 4D don't want to roll through time
    * added preference setting, how many dims to roll, which removes dims starting from first from the roll button and popup menu (clips to actual dims if higher)
        * (current) implementation: Dims class has extra poperty connected to settings, and the qt_dim_sorter to clip the available order
            * still some connections cause breakage
    * Andy: are you limited by how napari implements dims and how this could evolve
        * Chris: originally wanted to fix specfic dim, but could not implement it with the dynamic selectable evented list (the roll right-click menu), then decided to have it just affect the last dims (spatial should be last)
* Grzegorz: 0.4.18 release state
    * hopefully all PR are cherrypicked
    * need to update affiliations for citations -- what should the order be?
    * will make repo with scripts for future releases or other projects
    * don't install using pip git+ because it will use lfs
    * rc1 probably tomorrow 
    * 5963: numpy maybe broke tests (async)
    * Peter: what about docs repo and the split?
        * Grzegorz: cherry-picking BACK to napari/napari (0.4.18 tag in docs)
* Wouter-Michiel: layer.data.events documentation
    * At the moment documentation of this and other parts is missing due to autogeneration of the documentation in _scripts. Are there possible ways to get around this?

## 2023-06-07
* Grzegorz: Start 0.4.18 release process https://github.com/napari/napari/issues/5911, https://github.com/napari/napari/pull/5913
* Sean: are these plugins not installable [errors.json](https://npe2api.vercel.app/errors.json)?
* Grzegorz: https://github.com/napari/napari/pull/5908
    * Potentially add info about the new disable throttling fixture to the docs repo (in the contribution guide) as well in a separate PR.

## 2023-05-24

* Intro: Aakash Mahalingam - CZI software engineering intern for summer 2023
* Jordao: graph layer PR questions, https://github.com/napari/napari/pull/5861
    * what should be the points edges name in the napari graph layer? `edge_` -> `border_`. Vector and Shapes also have this attributes.
    * optional dependency to napari-graph? it avoid making numba optional. Make it optional.
    * some docs aspects remaining
    * the napari-graph package is a data structure similar to mamut
- Sean: magicgui dropdown with lineedit to add items gets reset with adding layers
    - Grzegorz: reset method is called when layers added, which is autogenerated by magicgui (think comboboxes for layers), see:
        - https://github.com/pyapp-kit/magicgui/blob/32ca80dfd34b8bdb914fcdc3bd37b31f8f485baa/src/magicgui/widgets/bases/_categorical_widget.py#L85
        - https://github.com/napari/napari/blob/0bc1adc290d50f9793bd1b7c0ce912095f2e2354/napari/_qt/qt_main_window.py#L946
        - Related https://forum.image.sc/t/resetting-choices-in-select-widget/63545/2?u=psobolewskiphd
- Grzegorz telemetry NAP
    - key to easily disable and user control of level of telemetry
    - plugin and contribution use -- could make public
        - usage: dask? zarr? numpy?
    - napari error reporter -- detailed information and frequently bugs of plugins not napari
    - focused on information about usage at the moment
    - Peter: should consider who will have accses to what data and how long will be stored (raw vs aggregated) 
    - Ashley: maintenance and cost of the remote side if it's ongoing
- Overall there the ability to ask @napari/copy to help clean up docstrings and PR descriptions - see an example here https://github.com/napari/napari/pull/5804, the team is viewable at https://github.com/orgs/napari/teams/copy for napari.org members. This might change in the future to a label or to a bot.

## 2023-05-10
- Wouter-Michiel: [Auto-fill' labels or 'fill contour' mode](https://github.com/napari/napari/pull/5006) in context of recent work on polygon lasso shape / label
- Wouter-Michiel: zoom while drawing shapes
    - With https://github.com/napari/napari/pull/5806 provides one way of zooming while drawing polygons. There is another way that could be tried which 
      would stick more to what originally was intended for allowing zooming while drawing (holding space bar).
    - Grzegorz: we should split zoom from layers, should be state of viewer. then enter zoom, store context of layer, then restore exit. Zoom doesn't move the layer, just camera.
    - Peter: Should tools be less layer specific? E.g. you could use the lasso tool in the shapes layer or labels layer in the same way. Perhaps overlays could be use to achieve this - the user drawing is kept as an overlay until the user clicks or presses space etc.
    - Grzegorz: Scope here to be more flexible on the shape layer, to move selections of shapes to different layers or convert selections of shapes to labels. Perhaps labels could also be converted to polygons. Plugins are not consistent in usage of labels, shapes, points (multiple labels vs multiple layers of labels). By having multiple layers you can have labels that overlap, which you can also have in the shapes layer with overlaps.
    - Peter: #5006 would be really nice to get in, so need to see about it getting finished, also UI aspect (Grzegorz)
- Walkthrough of progressive loading from Kyle [here](https://github.com/napari/napari/issues/5561). Could this style of walkthrough be achievable for other parts of the napari code base? Pre-recorded walkthroughs are likely good here. Peter - possibility to use the napari youtube channel for this purpose too, and link to it from docs.
- Wouter: layer groups and multicanvas view uses cases
        SHould it be part of napari core that if you have multichannel samples and layer groups you could store/preset certain layer groups for multiple samples for example
        Grzegorz: maybe this is like [Spaces NAP (NAP 3)](https://github.com/napari/docs/blob/main/docs/naps/3-spaces.md)?
        Grzegorz: easier if napari has API and plugins implement? otherwise could be a big UI element for something that may be rarely used
- Layer groups have unique layers in them, as opposed to sharing layers between groups.
- Peter: you could look at other programs that handle layers for inspiration to get a feel for what would not be unexpected to users (broadly understood) in terms of how layer groups are handled. Overall there are good ideas here about views, and layer groups - but getting the UI right may be tricky.
- Geneva: napari-lf plugin created and has a few questions about this.
    - The dropdown menus are glitchy (perhaps dependant on the computer)?
    - Can the cursor change in appearance when hovering over a button? Qt does have [cursor style](https://doc.qt.io/qtforpython-6/PySide6/QtGui/QCursor.html#detailed-description).
    - Can the slices be viewed more spread out from eachother? Perhaps scale could be used here to help this. Like `layer.scale = []`. Also the [api docs](https://napari.org/stable/api/napari.layers.Layer.html).
    - This plugin is implemented directly in QT by the looks. 
- Sean: demo: run napari from a script in VSCODE in debug python file mode, then breakpoints will work!!  
- 
## 2023-04-26
- Grzegorz: Compiled backend on example of PartSeg https://github.com/4DNucleome/PartSegCore-compiled-backend/pull/3
    - Any experience with compiled languages to improve the speed of certain operations in napari that are slow and regularly used (e.g. np.unique)?
    - Main issue is likely with maintainability.
    - However, if such an option is chosen, the package would be napari-compiled-backend or similar.
    - Then potentially, the napari-compiled-backend could be used for increased speed if it is available (similar to optional numba).
    - Pandas unique actually might be fast enough for this particular case (polars fast too, but would be extra dep).
- Lorenzo: 
    - [small update to napari-svg](https://github.com/napari/napari-svg/pull/27) (other plugins that are using 2d point size will break)
    - [big changes to dims still needed](https://github.com/napari/napari/pull/5751)
        - Is there a possibility that changing to world space instead of slider space will cause more issues than were intended? (e.g. in nm scale having issues with floating precision).
        - Andy - strong consideration towards a dims being a candidate for redesign. So this could be a first good step to doing that. Link to some dims issues [here](https://hackmd.io/6ZDQtpwzS5Sr_J71KsTehg). Could also look to ITK and VTK for inspiration.
    - [euroscipy](https://pretalx.com/euroscipy-2023/cfp), pssible funding (last year's page): https://www.euroscipy.org/2022/finaid.html
- Kyle:
    - advice on easy PR [https://github.com/napari/napari/pull/5759](https://github.com/napari/napari/pull/5759), fails on only `PR Test / ubuntu-20.04 3.8 pyqt5 min_req`
    - Might be useful to try https://pypi.org/project/tox-min-req/ to help with local testing or to try https://github.com/nektos/act
- Jaime:
    - Help with some flaky tests on macOS? https://github.com/napari/napari-plugin-manager/pull/3#issuecomment-1521684031
    - This could be related to different scheduler behaviour on the different OSes. So might need some waits (waitUntil all elements added to plugin manager).

## 2023-04-12

- Andy: PR for removing old async/octree code/tests
    - https://github.com/napari/napari/pull/5711
- Lorenzo: thick slices are happening!
    - https://github.com/napari/napari/pull/5522
    - https://github.com/napari/napari/pull/5697
- Jordao: napari-graph, next steps?
    - Should likely be an optional dependency.
    - Currently, the API for people making their own layers is not stable - they have to implement some private methods (which are then changed in the backend very regularly, as they are private).
    - Possibility to have two versions of the graph layer, one using numba and one not.
- Lorenzo: possibly tricky PRs
    - Point size no longer anisotropic: https://github.com/napari/napari/pull/5582
    - Throttling all mouse move events: https://github.com/napari/napari/pull/5710
- Grzegorz: pinning test dependencies
    -  https://github.com/napari/napari/pull/5715
- Jaime: plugin manager as an external package?
    - https://github.com/napari/napari/issues/5327

## 2023-03-29
- Ashley: looking for co-maintainer for [headless-gui](https://github.com/aganders3/headless-gui) (needs reviewers)
    - Kyle: what is it doing?
    - Ashley: it simplies workflow files (Linux)
    - Peter: can this type of vitural frame buffer trick be used to run tests or build docs locally?
    - Grzegorz: on linux it works using `xvfb-run`
        - maybe for docs we could add flags (e.g. `no_regenerate_examples`) to not (re)build examples and speed up the building of docs and avoid stealing focus etc
    - Ashley: maybe `QT_QPA_PLATFORM=offscreen`
    - Kyle: could this use conda? would get me interested
    
- Andy: failing PR action pyside6
    - https://github.com/napari/napari/issues/5657
    - could pin pyside6 in tox
    - Grzegorz: could use a constraints file, updated weekly or such
        - Pro: avoid breaking PRs because some dependency has a bug that needs to be fixed. Enables better env reproduction
        - Con: will require regular PR review on this file, e.g. weekly
        - Related example in [PartSeg](https://github.com/4DNucleome/PartSeg/pull/917)
    - Peter: the tests fail when run as a suite, but pass when run individually

## 2023-03-15

- Ashley: bug report about Surface dimensions [#5620](https://github.com/napari/napari/issues/5620)
- Ashley: thoughts on API for Surface layer textures and colors
    - support multiple textures? texture coordinates?
    - higher-dimensional `vertex_colors`?
- Neale (moseyic on image.sc): QoL issue low fps (5-8 fps) i7, nvida 3070 (https://forum.image.sc/t/poor-3d-viewer-performance-in-napari/68103/6)
- Neale: rendering RGB (zyx by 3, continous color data) on 3D-rendered ISO surface (https://forum.image.sc/t/napari-colormaps-from-fiji-imagej/77764)
    - Lorenzo: vispy doesn't have support—Talley intended to work on this (https://github.com/vispy/vispy/pull/1999)
    - Lorenzo: could split the channels, but don't know what to expect for ISO surface or use MIP and contrast limits or try Attenuated MIP
- Peter: looking for feedback on UI bug [#5629](https://github.com/napari/napari/issues/5629): dragging layers to the trash button
- Peter: new Plugin Manager: can anyone test without conda or conda env? See discussion started [here](https://github.com/napari/napari/pull/5622#issuecomment-1465286183) and issue (❤️ Ashley): [#5633](https://github.com/napari/napari/issues/5633)
    - Lorenzo: non-conda env, conda is offerred and clicking install gives errors
    - Grzegorz: same
- Peter: should we add napari[pyqt6] and napari[pyside6] as install options
    - Lorenzo: as long as clear it's experimental!
    - Andy: is there a goal to transition to Qt6 so we dont support both?
    - Grzegorz: this is my goal, but not sure when
    - Andy: LTS for Qt5 https://www.qt.io/blog/qt-5.15-extended-support-for-subscription-license-holders

### Action item: contact Pam, Gonzalo, and Jaime re: Plugin Manager issue

## 2023-03-01

- Grzegorz: Request opinion for [#5589](https://github.com/napari/napari/issues/5589)
-- Peter: concerns about in-flight PRs (but can update toml with ruff)
-- This is [an example](https://beta.ruff.rs/docs/rules/#flake8-pytest-style-pt) of a good styling but would make a [lot of changes](https://github.com/napari/napari/pull/5590/files#diff-cc0238fb1d9116e4f7abc91ad43bfe1d3bbd62705e798782ce70a5551cf2b972) and need a PR by itself.
- Wouter-Michiel: discuss way to add pencil support and exposing epsilon parameter of RDP algorithm for [#5555](https://github.com/napari/napari/pull/5555).
-- Peter: checkbox for disabling deletion of points (e.g. RDP algorithm)
-- Kyle: note that `epsilon==0` means RDP won't delete points [verify with implementation]
-- Grzegorz: use help text to explain the feature. Consider putting this into settings so it can be persistent across napari sessions (the settings is currently the only place to store persistent in napari). The settings can be linked to UI in other places also. Put it into the experimental section of the settings. Suggests, right click on dialog to change settings.
-- Having another icon for this functionality vs keeping the triangle icon for drawing polygons. Is it intuitive to switch between the two modes? Perhaps a setting could be designed to change how to mouse functionality for this works (e.g holding down mouse button draws points vs points draw when mouse button not held).
- Peter: Gestures in napari, though it should be possible in theory with Vispy, there seems to be some errors with this in napari. However, we probably need some input from users that actually use gestures (e.g. with of the touch screen laptops).
- Joel: Will napari be pip installable on Mac (ARM)? It should hopefully be in about a month with QT6.
- Wouter-Michiel: discuss [#5558](https://github.com/napari/napari/pull/5558). Should we provide the proper fix and close this PR for now or do we provide a relatively quick workaround and then right away work on proper fix (this was not addressed in this meeting in the end).

## 2023-02-15

- Wouter-Michiel: Qt/vispy decouple PR
    - Implemented fixes for Lorenzo's and Grzegorz' comments.
        - E.g. backward compatibility for QtViewer (partseg does this, and others do too probably).
    - Please take another look, this could be close to merge.
- Ashley: communication with napari as an installed program
    - https://github.com/napari/napari/issues/5563
    - plugin vs. core?
        - plugin not viable right now because users need to take an action to activate/start a plugin
        - could allow plugins to start at initialization time (but shouldn't be common, on by default, and always in control of users)
    - peter
        - console is already a plugin, but in the napari org, and has some elevanted privileges as a result
        - could this plugin be treated in the same way?
        - ashley: maybe we're trying to move away from these special cases?
    - grzegorz
        - plugin first to get a better understanding of what this is
        - not a great time to try to add to core given slow release cycle
        - manually doing some things at startup (e.g. opening plugin dock widgets, rearranging them), could be generally useful
- Joel: how to close a plugin dock widget programmatically
    - grzegorz: add_plugin_dock_widget should always return the reference to the widget (even if it already exists), this can be closed
    - fixed during meeting!
    - peter: should we add an issue to improve this?
    - grzegorz: maybe `viewer.window.remove_dock_widget(self.native)` could also work
        - also solves it and maybe more elegant
- Grzegorz: not napari, but related
    - https://github.com/Czaki/tox-min-req
    - minreq failures on napari CI
    - started working on tox plugin to pick up min requirements from python package files (e.g. setup.cfg, pyproject.toml)
    - difficult to work with transitive dependencies

## 2023-02-01

- Grzegorz: highlight ruff changes
- Kandarp: survey discussion (analysis requests; other questions of interest)
    - https://bit.ly/napari-survey-sheet (comment on sheet or email [me](mailto:kkhandwala@chanzuckerberg.com))
    - Last year's analysis linked here: https://twitter.com/jnuneziglesias/status/1613839615030628353
- Kyle: interest in pyinstaller alternative for napari and other related packages?
- Ashley: support for Qt6?

## 2023-01-18

- Annual survey open for another week - don't forget to fill it out!
    - https://bit.ly/napari-survey-c
- Lorenzo: [overlays PR](https://github.com/napari/napari/pull/4894): probably will merge soon, check on it!
- Grzegorz: upcoming napari releases
    - If you want something to be included, now is the time to mark it with the 0.5 milestone and get it ready to merge into `main`
- Andy: early feedback on dimension metadata
    - consider adding dimension types and units to viewer model
    - inherit scale (and similar) from existing layers when not specified by a plugin
- Peter: how magical should napari be?
    - blending modes dependent on canvas color
    - slightly magic fix: https://github.com/napari/napari/pull/5487
    - additive blending scales the existing color based on the alpha channel
- Grzegorz: https://github.com/napari/napari/pull/5451
    - looking for feedback on desired behavior/design (not so much the code)
    - where to put `ViewerModel.help`
        - use an icon to show help when needed (not discoverable?)
        - help message not needed when hovering over the canvas?
        

## 2023-01-04

- Kyle discussed progressive loading plans
- **This was a short meeting**



## 2025-04-23

- Attendees: Alexandra Irger, Carol Willing, Eric Moerth, Etienne Doumazane, Wouter-Michiel Verdag, Grzegorz Bokota, Kyle Harrington, Peter Soboloweski,  

### Topics

- napari 0.6.0rc0 released 
- Introduction guests from [Vitessce](https://vitessce.io/)
    - Main interest in progressive loading example from Kyle
    - Kyle - showcase of Vitessce and napari both being able to show the same data would be an easy touchpoint
    - Eric - at what level could we combine forces?
    - WM - should take into account that Vitessce is WebGL which is more constrained than OpenGL which is what napari uses.
    - kyle - chunk prioritization and fast-api to serve the zarrs is something we could collaborate on. Other option is also the view configurations that Wouter-Michiel is working on.
    - Shared in chat by Kyle https://github.com/napari/napari/issues/1019#issuecomment-2773741109
    - Grzegorz - suggestion to share tests is a postive step
    - Alexandra - Create texture objects for GPU to pass info to CPU

### Action points

- [x] Kyle created the working-group-multiscale-data channel on the napari zulip: https://napari.zulipchat.com/#narrow/channel/499584-working-group-multiscale-data
- [ ] Kyle and Eric create test repo uniform server with different endpoints


## 2025-04-09
- Attendees: Wouter-Michiel, Lorenzo, Tim, Kyle, Grzegorz
### Topics 
- [Lorenzo] discusses next plans for working on napari - especially instance rendering, using napari for rendering molecular dynamics
- [wm] small demo web dashboard in napari
    - Working on integrating napari with things like vitessce
    - Working on storing a 'plot state' to re-render visualizations in different sessions
    - Discusses using pyqt-graph, fastplotlib, holoview, etc to accelerated dynamic plotting compared to matplotlib
- Getting the docs closer to napari
    - Grzegorz says that Sphinx docs can be compiled to native Qt, but this may not work with all our Sphinx extension
    - Tim: Some parts of the API are missing, like overlay things (TextManager, ScaleBar, etc)
    - Grzegorz: We have to just sit down and hook it up to the docs
    - Qt uses webkit, which is not the greatest web browser -- so our native docs may not work correctly as in a browser format, so Grzegorz suggests trying to build from Sphinx to [QtHelpBuilder](https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinxcontrib.qthelp.QtHelpBuilder)
    - WM discusses that PyQt5 is worse for web rendering that Qt6
- Grzegorz finds PyQt6 is still flaky, but this may be helpful in pinpointing where our test flakiness lies.
    - Eventually, Qt5 will be dropped as the last supported Python version is dropped (Qt5 no longer builds for new pythons)
    - Pyside6 is already on [conda-forge](https://anaconda.org/conda-forge/pyside6), so once that is working we may move to there for licensing reasons
- [Tim] How would you show off napari in 5 minutes (doing a 5-minute tutorial/preview/talk for Bioimaging North America Early Career Workshop on bioimaging tools)
    - Kyle - talk in Barcelona, the most hackable viewer, brings a lot of interactivity
    - WM - Ipython console connectivity
    - WM - photoshop style of layering
    - WM - magicgui widget -- Tim will show off napari-plugin-template
    - Gzegorz - the great visualization of napari, 2D/3D, fast jump into multiscale
    - Grzegorz - we are not a bundle like FIJI, but we do easily bring in plugins
    - Weaknesses - instability of API, and older plugins are not working, our team is focusing on development of the workflow
    - In FIJI world, you can fire and forget, where y ou publish and move on, FIJI has technical debt, whereas we are trying to keep a good test suite to prevent breaking things (for readers, this is not meant as a slight against FIJI, we were just discussing the different strategies)
    - https://github.com/pymmcore-plus/napari-micromanager
- [Grzegorz] getting polygons with holes finalized, Tim will take a look for readability



## 2025-03-26
- Attendees: Carol, Grzegorz, Kyle, Etienne, Tim 
### Topics 
- [Grzegorz] - bermuda status
    - close to 0.1.2 release 
    - close to replace PartSegCore-compiled-backend with bermuda
    - Goal is to include bermuda in 0.6.0a1
- [Etienne] shows rendering of many points (from Spatial Data on a brain slcie) and shows issues with changing point size
    - Etienne notes that points dissapear upon scrolling out, that point size can update very slowly 
    - Etienne would like to change the range of the points slider, such that it behaves similarly to contrast limits
    - Grzegorz discusses the `canvas_size_limits` property of Points Layer
    - Tim sees that the documentation of this is minimal in docs, will open an issue
    - Points Layer has a size limit Grzegorz says is mac specific 
    - Kyle shared [#6148](https://github.com/napari/napari/issues/6148) for more context 
    - Grzegorz shares a [possible limitation to point size](https://github.com/napari/napari/blob/6f702fe757610fbf29e91db787f3f38cfa4daa15/napari/_qt/layer_controls/qt_points_controls.py#L116) - Tim notes [#4951](https://github.com/napari/napari/pull/4951) implemented this. In order to dynamically adjust slider with given points value
    - Grzegorz brings up need for issue to provide frozen data layers -- so that data can be inspected but not modified. Some features could still be edited, such as point size. 
- We start discussing users and their inhibition to restart napari -- Tim finds some users with 100s of layers open (often weeks of work) because they are too afraid to restart
    - Grzegorz suggests way to help users could go in a blog post
    - Grzegorz suggests changing color of terminal window
- Carol asks if we can save a snapshot of the napari state, nor the ability to save projects. Draga / Kyle may have the most context on state of serialization
- Carol discusses new goals with Steering Council to generate a new roadmap, with areas of focus 1) core API 2) UX / GUI, and plugins 3) things needed for maintainability, sustainability, documentation. Plan is to finish the draft document, and then share with wider core team. One goal is getting this information to funders. 
- Discussion of cloud stuff
    - Tim shares https://www.nimbusimage.com/ which is built by KitWare 
    - WM shares http://vitessce.io/

### Action Items
1. Tim open issue to document `canvas_size_limits` and purpose, since info is limited
2. Etienne open issue to discuss advanced point size slider
3. Etienne open issue to discuss providing frozen, non-editable, data layers
4. Etienne open issue to propose global editing of points without selecting them.


## 2025-03-12
- Attendees: Grzegorz, Tim, Carol, Etienne, Elise, Romain -- introductions were shared with everyone
- [Romain] [BIOP-desktop](https://biop.github.io/biop-desktop-doc/)
- Multiple layer status information [#7673](https://github.com/napari/napari/pull/7673) *Carol - I moved to top of agenda in case WM can make the meeting*
    - Romain does not prefer the space ('    ') appearance; fniding actual unicode separators as more readable
    - Romain wonders about making other information available in the GUI, such as more information about the names
    - We discussed floating out widgets of the usual positions, which was extended to interest in maybe making multi-column widgets (like moving the layer list adjacent to layer controls)
    - Romain requests a shortcut to switch between layers while in the canvas, instead of in the layer list -- Romain will open an issue for this request. Tim shared [#7447](https://github.com/napari/napari/issues/7447) and [docs#538](https://github.com/napari/docs/issues/538) for context. Shft+Alt+Up/Down works to move between layers, but only then shows them. Will find this implementaiton
    - Etienne preferred the space separator for status bar
    - Etienne suggests eliding long names in the status bar 
    - Etienne suggests adding the scale to position in status bar
    - Grzegorz discusses ned to change position of status bar to floats
    - Tim: Open request in napari to improve display of layer metadata in the GUI (mostly only accessible quietly)
    - Etienne encourages keeping the status bar as simple as possible, but exposing more layer metadata information elsewhere (scale, transforms, etc)
    - Etienne recommended exposing as a GUI preference the status bar separators, under the impression that it will be settable
- Review [release plan for 0.6.0a1](https://hackmd.io/@willingcn/SJgmBTnjJe/edit) with community - Carol
    - We did not specifically discuss this release plan. However, we did discuss the release schedule and testing out the releases and giving feedback on Github or Zulip 
- (not discussed) Tagging PRS with social media label - WM if I can make it, last minute teaching
    - Main idea is that I think we should tag PRs with social media label if we can create an interesting example with it or create a story out of it to show on the socials. This can then also serve to give bigger specific recognition to community contributions and we can already point people to the fact that they can test it out on main. Would love to hear your opinions. Note that it would require permission I think of the person who created the PR.
- (not discussed) Adopt codespell to our codebase [#7619](https://github.com/napari/napari/pull/7619)
    - Recommend we pause this PR until after the 0.6.0 release to avoid introducing inadvertent code changes. *Carol*

### Action Items
1. Romain: Open request about switching between layers from the canvas
2. Tim: Open request to improve display of layer metadata in the GUI (scale, transforms, reader, etc)
3. Tim: Add notes from community meeting to status bar PR [#7673](https://github.com/napari/napari/pull/7673)

## 2025-02-26
- wm: If Tim is also present I would like to discuss using vega specification as much as possible to improve on font size, positioning
of overlays etc.
    - actionpoint wm: bump scale-bar pr and open issue with example overlay configs with vega spec. 
    - Grzegorz: would not be good to work with jsons.
    - WM: would be more to use the same grammar of graphics where possible and also to check consistency across the viewer when we have settings related to 
    positioning, width, thickness etc.
- Grzegorz: Discussed [PR#6986](https://github.com/napari/napari/pull/6986) about inheriting spatial info for functional widgets that return on layer data
## 2025-02-12

- Discussed grid spacing PR (https://github.com/napari/napari/pull/7597) and noted some minor improvements
- Discussed exposing canvas color in the public api and gui (turns out canvas affects blending in grid mode!)
- :point_up_2: Add agenda items here!
- Grzegorz: 0.6.0 release plans
    - Tim: when?
    - Grzegorz: ASAP
    - Peter: we need to figure out any key docs things that need to be co-released, for example anything related to python version. Also double check the UI changes documentation.

## 2025-01-29

- Grzegorz: Polygon with holes [#7566](https://github.com/napari/napari/pull/7566)
    - Use case driving this: performance for spatial applications
- Grzegorz: updates on bermuda project: https://github.com/napari/bermuda
    - Rust performance acceleration for spatial algorithms
    - Coming along nicely; working toward initial release
- Discussion of mypy usage for typing of elements in napari core depends on typing prototypes of numpy, dask, zarr, etc.

## 2025-01-15
- Grzegorz: release 0.5.6 updates
- Etienne: toggle visibility should maybe autoselect the affected layer? Opening issue...
- Timothy: indicator for context menu [#7502](https://github.com/napari/napari/issues/7502)
    - update help info/docs with more info/emphasis about context menus
    - update the empty canvas welcome-widget with some extra info ("tips and tricks")
- Timothy: scale bar and over overlays text: connected to font size settings, or extra setting for default font size?
- Timothy: clarify [architecture doc](https://napari.org/stable/developers/architecture/index.html) 
    - Model View Controller
![image](https://hackmd.io/_uploads/Skz-RDHwkg.png)
    - Making separate type hint PRs is useful
    - Using IDE debugger to track objects created
- Code café

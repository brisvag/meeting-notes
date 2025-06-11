## 2024-10-16

- Present: Grzegorz, Draga, Juan, Etienne, Wouter-Michiel and Carol
    - Welcome Carol and Etienne!
- Introductions
- [WM] Refactoring shapes & labels
    - want to start on machinery for reusing painting in shapes and layers
        - move out the mouse moving machinery
            - DDP: points also has some mouse moving machinery and events
                - could be worth keeping it in mind as we split out shapes and labels
        - shapes uses triangulation and labels uses overlay for drawing, WM wants both to use for overlay
    - do we want to expose via GUI the option to go back to old code?
        - no, happy to go with just removing the old code
    - JNI suggests starting with a shared tool like paintbrush in shapes or lines in labels 
    - GB recommends starting with implementing both as overlays first, and make it a breaking change
        - drawing might be a problem because with labels you can easily join separate "polygons" with multiple strokes, but that's not easy with shapes because on conversion it might turn into sets of polygons and you've lost the semantic meaning
    - JNI: Add support for points to Shapes layer, since GeoPandas DFs include Points in geometries
- https://github.com/napari/napari/pull/7332
    - if there's no test before we start on release, we'll merge without test to fix the catastrophic bug
    - otherwise GB will work on a test, ideally
- https://github.com/napari/napari/pull/7268 
- Blog post Grzegorz wrote on preventing segfaults in qt test suites
    - the detecting leaked widgets bit will be useful for napari blog
    - should expand with view to plugin developers
    - WM will review for language
        - https://github.com/Czaki/Czaki.github.io/blob/main/docs/blog/posts/pinning_dependecies.md
- :point_up_2: 
 

- add your agenda items here!


### Action items

- Juan: make issue about including points in Shapes layer data model
- 

## 2024-10-02

present: Juan, Grzegorz, Draga

- [Grzegorz] Walk through https://github.com/napari/napari/pull/7268
    - intersection finding algorithm requires ordered set, so there's still potential improvement using C++
    - doing it numba is possible but it's 5 minutes compile-time
    - currently the new triangulation is only used if you have numba, otherwise we go back to the original implementation which is numpy vectorised (so current implementation without numba would be v slow)
    - what's the problem with the current numpy vectorised version? somewhere in `_shapes_utils.py/generate_2d_meshes` there is reallocation of arrays
    - in the new for loop we use a `_set_centers_and_offsets` function which calculates miter lengths etc., and this would be a pain to vectorize
    - the pathing algorithm is the same as what's in the current codebase, and it's only been improved for better joins
        - haven't been referencing anything
- [Juan] current roadmap revisit?
- [Draga/Lucy] More keybindings questions
    - when should dialog pop-up
        - if installed from manager during session, should be asked immediately
        - if installing from pip/conda, should be asked on napari launch
        - generally Grzegorz is happy with early detection
    - w.b. keybindings registered as part of plugin widgets etc.
        - https://github.com/napari/napari/issues/7312
- :point_up_2: add your agenda items here!


### Action items

- Juan: thumbs up and comment on Draga's comment ([link:](https://github.com/napari/napari/issues/7312)) to say that we want declarative everything, and that the mechanism for declaring that a shortcut maps to a widget method and should only be active when there is a widget instance is feasible.

## 2024-09-18


- [Grzegorz] PR review
    - leaking widget PR https://github.com/napari/napari/pull/7251
        - Juan will update wording of a couple of comments and mark ready to merge
    - reset contrast PR https://github.com/napari/napari/pull/5136
        - Lorenzo previously had concerns about the performance impact of the PR, but Grzegorz pointed out that it's better to make sure that beginners don't get met with a blank screen and don't know why, as advanced users/plugin devs can always fix it
        - Lorenzo has been convinced by the arguments that it's better to have it than not - how can we warn users about potential impact?
        - only affects taking/returning **data** not widgets that take a full layer
        - Draga's concern is that the magic will behave inconsistently and if the magic goes wrong, it's even harder for a novice user to recover. Why not warn instead and tell the user to check the contrast limits
        - Lorenzo suggests just updating the range rather than also resetting the limits, because then at least the user sees the slider changing and can set to continuous if they want, or just move the slider
        - very wide ranging discussion about magic vs. no-magic, whether we should allow taking layer data at all, the state of the plugin ecosystem ... capitalism
        - ultimately we do agree the current state is problematic. Would like to shop around the idea more
- [Draga] Layer controls redesign
    - do we want to prioritise this?
    - https://github.com/napari/napari/pull/6219
        - Juan favours icebox
        - Grzegorz does want it done, but we need more core devs on it, ideally a hackathon but at least a PR party or similar. Would like to take a look at it but the time is not there and current priority is SDG. For following month Daniel should not concern himself with updating it
        - Lorenzo says it's high activation energy to look at, and it's qt stuff which makes it trickier. Don't expect to be digging into it any time soon
        - Potential for the PR to be split into smaller parts so that even if we don't get the whole redesign merged, we leave the layer controls in a better state
- [Draga/Lucy] Keybindings
    - Alternative keybindings - yay or nay? Cast your votes!
        - https://napari.zulipchat.com/#narrow/stream/212875-general/topic/Vote.20on.20'Alternative'.20keybinding
    - Plugins declaring same keybinding as default napari keybinding
        - what happens? need broad discussion
        - **Bumped**
- [Juan] napari-console 18: https://github.com/napari/napari-console/pull/18
    - I think we should just merge this as-is.

### Action items

- [Juan]: comment on https://github.com/napari/napari/pull/5136 with summary of extensive discussion :joy:

## 2024-09-03

- [Draga] NAP-6 next steps
    - opening up Plugins menu for contribution?
    - Plugins' home menu
    - https://github.com/napari/napari/issues/7012#issuecomment-2327873949
    - Discussion/conclusions:
        - general feeling is that we're happy to open up `napari/plugins` for contribution
        - the more discussion we have, the less concerned we are about the idea of plugins "hacking" other plugins menu
        - ultimately settled on plugin's own menu being contributable by default, and toggleable to non-contributable
            - also useful if a plugin is no longer maintained, as that way we don't need that plugin's manifest to change in order for someone to make a child plugin and still have it be very useful
        - the question of command duplication becomes more concerning once we open `napari/plugins`, but maybe it's not that big of an issue and we can punt on it until someone complains. Easy enough to change later
        - need to update the NAP to mention the ordering of things once we open `napari/plugins`: should be group of built-in stuff, group of plugin-declared submenus, final group of plugin home menus
        - we should make a big general comment about things being under review and experimental and we might move stuff around
        - w.r.t conflicts nobody was too concerned, but when it comes to the same ID, same location, different labels thing, it should be deterministic, not changeable depending on session
            - one idea was sorting by manifest last modified
- Some discussion on #5116 granular contribution enable/disable
    - agreed we really want it and will be very useful
    - definitely should be in settings because lots of people never even use the plugin manager
- Proposal for "orphaned" plugin org
    - if plugin is no longer maintained, author can "donate" it and relevant credentials to plugin org, we advertise it as orphaned, can transfer credentials and repo if someone else offers to pick it up for maintenance
- [wm] roi screenshots https://github.com/napari/napari/pull/7209
    - Contribution from community member
    - would be great to get review on it!
    - Discussion
        - other things to consider would be the size of the scale bar w.r.t the size of the image, and potentially capturing contrast limits as part of the scale bar
        - GB: do we want to use a shapes layer, because realistically you're only going to make rectangular screenshots so could be easier to make a dedicated interface for rectangles
        - WM: yeah, but sometimes in data you want to highlight circular regions etc. even if the screenshot is still rectangular
        - LG: was using overlays for measuring and folks were wanting something more persistent than the overlay, and the shapes layer would be really useful for that there because you have lines, rectangles etc. Feels a bit weird to "pass" the shapes layer, but it might be useful for the interface to just take bounding box ROIs. Beneficial to have a napari builtin that generates maybe a new layer for ROIs that you can then use for screenshots. It's messy generally that we have the shapes layer contain so many unrelated things
        - GB: people might want to provide ROIs in different ways than shapes
        - GB: PR that adds bounding boxes to shapes... #7144
        - GB: maybe best option is to have a light interface in napari and then keeping the ROI screenshot in a plugin?
        - LG/GB: programmatic interface for this shouldn't have to go through the whole plugin thing, we should just have programmatic API in napari to do it. interface aspect we can punt on
        - WM: would be awesome if there was some kind of comment/review on the PR itself
        - GB: I'll try to respond
- [GB] 
    - channle axis deprecation https://github.com/napari/napari/pull/6694 (collect opinion)
        - GB: need to sit down for this. general agreement is that we should deprecate merge and release
        - general agreement
        - LG needs to review!
    - custom c++ compiled package
        - more detail in last week's meeting
        - would be great if we could actually think about this and whether we want it
        - LG: yeah triangulation shouldn't go in vispy because we use it in other places and will need it regardless of the backend
        - LG: concern is maintenance and build system?
        - GB: build is not so bad, but C++/cython code yes would need.
        - DDP: **some** experience, not super concerned about that aspect
        - LG: also has some experience
        - GB: will also be necessary for final review for correctness etc.
        - GB: will also need testing of the PR for speedup
- [LG] been playing with getting a pygfx canvas in napari
    - kinda just plugged it in and started fixing what broke
    - ultimately ran into the event system which is chaos, don't know the order and how to properly grab the event
    - need to probably talk to pygfx folks and also someone else from napari who knows the event system better
    - GB: I know event system but focus maybe should be on napari-lite...
    - GB: big problems is keybindings is passed through the vispy canvas, which we're slowly working on by deprecating action manager
    - LG: not sure what events we're relying on coming from vispy
    - LG: events not really accessible in the viewer, happen on the canvas and given to the callback but if I remove the canvas I can't access anything
    - GB: connect to mouse event on vispy canvas and then there's a list of callbacks (stupid) that gets called - stupid because no weakrefs
    - LG: would like to keep working on it, might open a draft PR
    - WM also wanted it, if we prepare an actual work plan maybe we can do a kind of "feature for sale" fundraising

## 2024-08-21

- [GB] Speedup triangulation and `napari-core` package
    - issues with polygons. First is that it doesn't support polygons with holes, second is that it's very slow (e.g. with spatial data code it takes more than 2 minutes to load)
        - might also be an issue with not that many polygons but lots of points
    - there are faster algorithms, but may not be faster in pure python. For example SweepLine algorithm is very fast but it's not really vectorizable so we can't take advantage of numpy
    - tried to play with numba, but compilation takes 2 to 3 minutes which is only useful if you pre-compile code
        - looks like it tries to unwrap the for loop into just a list of commands, but the for loop is too long for this to be useful
    - with colormaps we initially wrote C-code but could "numbaify" it, but for this one it doesn't look like that's going to be useful here
    - maybe we need to implement in C++? compiled backend for napari?
    - alternative would be to upstream to vispy, but not many options for review and then if we switch to e.g. pygfx the problem is back
    - no real impact on distribution of napari, but will be somewhat increased maintenance burden for core devs
        - will need to recompile for new numpy version for new python versions
    - what else could this enable if we do go the compiled route?
        - colormaps still could be improved e.g. for direct colormaps if we know there's a finite set of colormaps we could find one mapping for the whole set. Also current numba code has performance issues on the first run while numba compiles
        - another example **might** be one big shape with lasso, we triangulate from scratch when adding the next point, which is very slow.
        - there's probably others because our code is better slower than wrong, so in many situations we recalculate a lot from scratch
    - pure python implementation will need additional data structure implemented as well

## 2024-08-07
- [GB] improve Performance of napari [#7158](https://github.com/napari/napari/pull/7158), [#7146](https://github.com/napari/napari/pull/7146), [#7144](https://github.com/napari/napari/pull/7144)
- [LL] Keybinding functionality, conflicts etc [#7056](https://github.com/napari/napari/issues/7056)
    - Discussion points described in [issue](https://github.com/napari/napari/issues/7056#issuecomment-2274825593).
    - GB mentioned a (PyQt?) issue where menu actions are queried first, before being passed to qt window? He will investigate and report back.
## 2024-07-24

- freehand drawing lines in shapes layer
- (Juan) cleaning up 0.5.1 milestone and pushing 0.5.1rc0
- (Grzegorz) dashboard tour

## 2024-07-10
Present:
- [WM] EuroSciPy 2024
    - Napari talk accepted for conference track
- [GB] Check milestone https://github.com/napari/napari/pull/7083
- [jni] settings schema migration https://github.com/napari/napari/pull/7053
- [Alessandro] (if time) quick demo of [group layer visibility toggle](https://github.com/brainglobe/napari-experimental/tree/ARC-dev-branch)


## 2024-06-26
Present:

- [Juan] NAP-6 ([#7011](https://github.com/napari/napari/pull/7011)) demo (basically, [look at this affinder PR!](https://github.com/jni/affinder/pull/96) :heart_eyes:)
- [Juan] 0.5.0a0 release ([draft release notes PR](https://github.com/napari/docs/pull/430))
- [Genevieve] alpha colormap question (re: https://github.com/napari/napari-tiff/pull/31)
- :point_up_2: add your agenda items here!

## 2024-06-12
Present: Grzegorz, Eric Perlman, Draga, Genevieve, Juan

- [Juan]: [NAP-6 updates](https://github.com/napari/docs/pull/312) from Draga
    - fully qualified vs shorthand/single name menus
        - people like shorthand in general
        - advantage: can move menus around without affecting plugin authors
    - self vs plugin name vs implicit
        - plugins can organise plugin>plugin-name>... as they wish
        - Grzegorz: copy-paste is important sometimes and could be broken by implicit names
        - metaplugin could declare a contributable menu, and other plugins could then add to those menus
    - edit / modify / in-place
        - guideline for plugins that if they modify layer data they should go here. Annotate / Transforms / ...?
        - data vs metadata
- [Eric] FYI ["NGFF Challenge"](https://forum.image.sc/t/ome2024-ngff-challenge/97363)
- [Genevieve] copier in napari-plugin-template, synching main branch with cookiecutter-napari-plugin repo
    - [Juan] is it hard to support both cookiecutter and copier in same repo? See https://github.com/scientific-python/cookie?tab=readme-ov-file#to-use-modern-copier-version
    - [Genevieve] will need to update PR to make it to original repo, or update new repo to bring it up to date with cookiecutter repo. Whichever is easiest.
    - [Juan]: will test out updating based on updated template.
- [Grzegorz] Questions about cachable context https://github.com/napari/napari/pull/6965
- :point_up_2: add your agenda items here!

A totally unnecessary limerick by Genevieve:
> During a difficult  code review
> You exclaim "I know just what to do!"
> It just goes to show
> That StackOverflow
> Holds all the right answers for you

## 2024-05-29
- [Juan] Adaptive rendering quality
    - Nice demo, right now when having larger step size, the intensity does drop. Unclear what the reason is.
- [Juan] aiming for 0.5.0 release soon!
- [Grzegorz] units metadata on layers
    - Units first, then layer inheritance
    - do we add a whole new class for axes and transformation metadata?
- [WM] screenshot PR functionality will be moved to new function export_views
- [Grzegorz] napari statistics dashboard https://czaki.github.io/napari_dashboard/
- :point_up_2: add your agenda items here!
 

## 2024-05-15
- [Jakub] Midi controller 🎛🎚️🥳
- [wm] Storing shapes
- [Draga/wm] automated testing of plugin errors for all plugins: https://github.com/napari/npe2api/blob/main/public/errors.json
- [Grzegorz] add test to cookiecutter to check napari.yaml is included in manifest

## 2024-04-16

- [Luca] [#6649](https://github.com/napari/napari/issues/6649) Points sizes
    - sizes not scaled with affine transform
    - difference between macos and other OSes for points geometries
    - work with v large images and points masks
    - their software supports points starting from 100s of millions, and sahpes with 100s of thousands of polygons (and circles, no ellipses)
        - circles can be turned into ellipses with isotropic transform
    - in napari with more than a few dozen thousand polygons, performance struggles
    - using points instead of circles to help performance, but that doesn't support affine scaling
    - would be great if shapes could be rendered in a more performant way\
    - if it's paid, how long would it take, what would be the path
    - also interested in rasterization of polygons (?)
    - JNI: few issues with polygons. The first is that when you're moving the cursor, you're updating what the cursor is saying. For shapes, this means we need to check if you're "in a polygon" with your cursor - very slow
        - if you deselect the layer sometimes it hugely improves performance
        - we also need to throttle this but hasn't been implemented
    - if still issues with deselect, limiting factor is vispy shader and opengl
    - would need someone who can look at performance of opengl code
    - also working slowly on decoupling layers from vispy so that you could use a different shader, but that's more on the order of 1-2 years
    - GB: there's also vispy events when moving camera (on the python side) and the number of events is nonlinear in number of objects. would show up during profiling. Definitely a python thing.
    - Luca: (demo) loading the shapes is v slow.
    - JNI: tried triangle?
    - Luca: yes, segfault
    - JNI: yeah I've run into that and I've got a branch where I'm playing with it but haven't got it finished. You can try the [PR 6654](https://github.com/napari/napari/pull/6654) with triangle possibly
    - JNI: currently triangulating over all polygons, but should be doing it async across multiple loops so that we can at least render as we go
    - Luca: deselecting layer helps
    - JNI: yep, [issue 5755](https://github.com/napari/napari/issues/5755) tracks the throttling need and has a few approaches there but we never got it off the ground
    - Luca: what would it mean for a contractor to work on shapes improvements
    - JNI: haven't really done the legwork on that. We really need someone who can actually do the work, I think the money will be easier to find. Right now for example we have a grant for instanced rendering, but we can't even find someone to take the money. So it's really finding the people that's the key issue
    - Luca: big maybe there could be money, but yes doesn't solve the problem of who would do the work
    - GB: this really should be split into two parts: first should just be debugging and investigating the exact root cause of the problem, and then fixing it. It may be that while debugging we find an easy problem, but even otherwise at least we could look for someone for specific experience with the actual fix. They're different needs for expertise.
    - Luca: points scaling with different types of machines
    - JNI: one issue is opengl, on mac you have a very narrow set of point sizes, and that's down to the opengl driver and we can't do anything about that. One option would be multiple backends e.g. use vulcan instead, or to use circles but you've already experimented with that. Potentially, if we improved shapes performance enough, that might solve it for you?
    - JNI: points will never do anisotropic scaling.
    - Luca: oh that's fine we don't really care about scaling, there's a minimum point size or something `layer.canvas_limits` that we use. But the points radii don't work
    - JNI: yeah ok so that you can't do. We need to either have a different backend on offer, or improve shapes performance. Lots of low hanging fruit for optimizing shape performance
    - Luca: multipolygons?
    - JNI: not supported directly, but with `properties` you can add for example a shared ID column that will enable many of the usecases for multipolygons
    - Luca: what if we back shapes with a Geospatial object that keeps a spatial index that we use for fast queries. Rasterize points on the fly based on the region you've queried, backed by a multiscale xarray
    - GB: yes, right now we don't have good data structures for querying points and shapes. The key issue is the data structure needs to be frequently updated. Ideally we could have something mixed with a proper index and an update list which only gets now and then.
    - Luca: for us, yeah, it doesn't need to be editable. Upon rasterizing it would be a labels or image layer, instead of a points layer directly. Any experience with on the fly rasterization?
    - JNI: we kind of do this with multiscale images already, slicing into it depending on the correct level of the array. If you had an arraylike structure, it should "just work" if you gave it to an image layer with the right API
    - GB: yes, would be many queries though. we depend a lot on caching zarr and dask arrays - if there's a big overlap in the slice, you really only slice the new bit. If you're doing your own dynamic loading, it might be much slower because you can't take advantage of that caching
    - JNI: could put a dask array in front of your image though, then you could take advantage of it
- [Grzegorz] [#6826](https://github.com/napari/napari/pull/6826) - pre release testing strategy 
- [Grzegorz] [#6827](https://github.com/napari/napari/pull/6827) Inheritance of unset parameters on new added layers.
    - for each layer, find all parameters that don't have default values
    - on new layers, set parameters to those non-default values
    - all old layers need to match on the non-default param in order for it to be inherited
        - JNI & Draga like this 
    - GB: maybe parameters should be treated differently e.g. scale is inherited in both directions, translate is only inherited in one direction. Axis labels for example - you'd expect they all land in the same coordinate space
    - 
- [Grzegorz] [#6775](https://github.com/napari/napari/pull/6775) Use ruff formater in place of black
- []

## 2024-03-19

- [Draga] menu contributions, the manifest structure and the next steps
    - there's some complexity about the way we register actions based on manifest contributions moving forward
    - in particular, `SampleContribution`s and `WidgetContribution`s are implicitly linked to `CommandContribution`s via the command id
    - as we look to expand the ways in which plugins can hook into napari, this implicit connection is going to cause some problems e.g. if a user assigns a `MenuContribution` to `command_x` and `command_x` is **also** a `WidgetContribution`, does that mean they always want `command_x` to add a widget to the viewer?
    - there's also some issues with the manifest structure both from the user's perspective (too much repetition) and from the parsing perspective (orientation is "contribution-first" rather than "command-first")
    - we need to decide what we want to do before release and what we want to do after
- [Grzegorz] - any remarks to [#6736](https://github.com/napari/napari/pull/6736) (initial PR for units)
    - [JNI] want a sequence for each type of transform so that it can be composite
    - [GB] I was thinking maybe let's just move existing system into separate class and do a round of deprecations (because layer code is very deep) first, because then we can move all the methods onto the object and edit there. For example what do you think about the order of the transforms?
    - [JNI] yeah I think it should be a list - explicitly because ideally people will then be able to put in their own transformation chains. Also it matches the ome model which will make it easier to load ome data into napari
    - [GB] yes but when we activate transform, we don't modify the early bits of the chain (scale, shear etc.), we do further up the chain - some of the transforms are data2physical and some are data2world
    - [JNI] in the ome spec there's a proposal for having multiple namespaces. Should be `CoordinateTransformation` and then within it there should be namespaces defining data2physical etc.
    - [GB] exposing the transform chain to users will be a nightmare. keeping named attributes will provide the order directly. A more advanced user could use the transform chain directly. The basic named attributes are good for interacting with the transform chain in expected ways
    - [JNI] I think it would be good actually if people could transform the data space in napari, rather than outside of it, which might also be difficult
    - [GB] what is blocking people from adding an additional matrix to the end of the chain? They just won't have correct scale info
    - [JNI] well you could do that now too but we don't have a good way for someone to read layers with metadata and have it set correctly
    - [GB] you could, if you return a layer. minus units, which is where this PR is going, but I want it in small steps so it doesn't get too big
    - [JNI] in that case I would reduce even further, so don't move the `affine` etc. out into their own object and just focus on the units, otherwise we're introducing a new API. It leaves us in a halfway spot where we'll still need to change the structure and everything. I want the concept of the spaces that we have in napari to be embedded in the object and explicit (data space, physical space, world space)
    - [GB] we could just start by exposing the transform chain itself and that's it
    - [JNI] don't think the spatial affine model is the right one because later it may not be affine
    - [GB] ok so maybe scale, rotate, translate we expose, but the rest should be redirected to the transform chain - because the first three are easy to understand and play with
    - [JNI] agrees. Would also maybe want to move towards pydantic models
    - [GB] I think in this case we should keep it classic. There's a whole bunch of machinery that needs to be explored
    - [JNI] works in pydantic 2
    - [GB] yes but then we have to build around pydantic machinery when we could just have getters and setters that validate correctly. maybe we should just build classic and then once we're happy with its form, see if it would be worth converting to pydantic

## 2024-03-05

- #4991 plugins menu
  - https://github.com/pyapp-kit/app-model/issues/52
  - https://github.com/pyapp-kit/app-model/issues/176
  - agreed we are not testing npe1 (Draga will test locally)
- python 3.8 drop?
    - any big PRs that we want to merge before we drop python 3.8
    - there will be some merge conflicts
    - Draga isn't aware of any but we should check with core devs
- 

## 2024-02-21

- Grzegorz: Final decison about 0.4.19post1/20 https://github.com/napari/napari/tree/v0.4.20x
    - it's all ready to go (three PRs to core and two to docs)
    - just need to decide what to name it - only Juan had strong opinions
- Grzegorz: new labels layer https://github.com/napari/napari/pull/6190
    - high on the priority list
    - deciding on backend array based on image layer
    - improving scale computation
    - both are connected to chunked data
    - GB: how much magic should there be? Right now there's three policies and we folow any existing image layers' class. But what to do with multiple image layers etc.
    - DDP: I think some level of magic is necessary, but the guessing should be kept to the minimum (and obvious). Any more complex stuff should be in a Layers -> New dialog (after NAP6) that provides the user full control over what type of layer they want and its config. Could also allow for plugin contributions. Should save frequently/recently used config
    - GB: right now a lot of the magic is to support different array interfaces, work out chunk sizes, etc. it's a lot of messy spaghetti code - not sure if there's better ways to do it
    - GB: ok proper solution after discussion: reduce magic; new labels should only work when there is consistency between layers (same shape, same type, same chunk size) and then we add a right click to the image layer that generates a labels layer like the image layer
    - GB: But, if we do the above, this will break use case of one imge layer (100, 100) scale=2 and another (150, 150) scale=3 which currently would give us a labels layer (600, 600) scale=1
    - DDP: well ok yeah that does sound reasonable... not sure where to draw the line
    - WM: yeah complex use cases we don't open from the viewer
    - DDP: maybe Peter would be good to ask?
    - AF: yeah our plugin has code in it to roughly guess. The user selects an image from a dropdown and then we do a bit of guessing logic and open a new labels layer for them. We only guess for numpy arrays though. I'll ask other folks using the plugin
- Grzegorz: approved edge to border renaming PR
    - just few small comments
    - Draga will follow up
    - (then update graph layer)
- Juan: shapes with holes in them 🍩 ([#5673](https://github.com/napari/napari/issues/5673)) now fixed when using triangle ([#6654](https://github.com/napari/napari/pull/6654)).
    - WM: spatial data folks want to maybe hire a contractor for shapes performance improvement
    - GB: might want to look at more psygnal events that could be slowing us down (like #6275) before handing off to contractor. evented set for example still has multiple places to fix
    - WM: ok will get back to this after spatial data release
- Draga: Plugins menu [#4991](https://github.com/napari/napari/pull/4991) once again ready for review - now with npe1 :octopus: 
    - Draga is good with changes
    - Grzegorz will follow up
    - code is untested - Grzegorz highlights this is not great. Draga said she's happy to do manual testing of the key npe1 workflows ahead of release. Don't want to spend time creating npe1 test fixtures just to remove it in the next version
- WM: automated message for first contributors using [action](https://github.com/actions/first-interaction)?
    - DDP: obvious concern is if they get the auto message but nothing else. Could we like make an automated message that pings after a week if there's no comment
    - GB: yes but much harder
    - WM: could also just be some general housekeeping stuff pointing people to community meetings, zulip, asking to ping core devs if they dont hear back
    - DDP: yeah that does make sense and it's only for first time contributor so it's not like they'd be spammed with it
    - WM: ok will open PR and post to core devs zulip
- Alessandro Felder (AF): quick question around napari dask support
    - GB: we support dask, zarr, tensorstore and xarray. the main problem is we don't read metadata about axes, order, colors etc. If all of that information can be packed into an xarray we should be able to handle that.

## 2024-02-07

- Juan-Wouter: SpatialData, no current OME-NGFF proposal, Wouter uses GeoPandas/GeoJSON for now, but no plugin for saving.
- (Draga) Menu contributions, npe1 deprecation and app-model conversion
    - `npe2 convert` command can help npe1 plugins migrate (still many missing)
    - shimming/convert doesn't work for multi-layer writers; but there is [documentation](https://napari.org/stable/plugins/npe2_migration_guide.html) about how to convert.
    - plugins menu depends on npe1 deprecation: https://github.com/napari/napari/pull/4991
    - menu contributions are currently built on top of the plugins menu -> app-model work
    - sharing TODOs
    - figuring out prioritization & dependencies
- (Draga) multicanvas...?
- (Juan) shapes with holes in them 🍩 ([#5673](https://github.com/napari/napari/issues/5673))
- 
- 👆your item here!
- https://github.com/napari/napari/pull/6190 - PR for new labels layer
- https://github.com/napari/napari/pull/6216 - PR for better min/max 

### Action items

- Draga: Will update menu contributions to not depend on [#4991](https://github.com/napari/napari/pull/4991)
    - will message in core devs with the plan
- We'll release 0.5 with menu contributions but not deprecating npe1
- Grzegorz will deep dive on [#4991](https://github.com/napari/napari/pull/4991) to see if we can somehow merge while still keeping npe1 widgets
- [NAP-6](https://napari.org/stable/naps/6-contributable-menus.html): Juan to review ([napari/docs#312](https://github.com/napari/docs/pull/312))

## 2024-01-24

- PR for 0.4.19: [#6616](https://github.com/napari/napari/issues/6616)
    - Draga and WM confirmed it fixes issue and approved PR
    - Test can come later
- Alessandro gave update on layer groups
    - merged main into original PR, ran into some issues
    - GB suggested potential source of error: adding Layers to layerlist instead of wrapping them in `Node` first
    - GB mentions that since `LayerGroup`s can override attributes of group nodes, we would also need to update e.g. giving plugins layers (because a layer whose visibility attributes are controlled by a group might have incorrect information)
    - GB suggests maybe developing `LayerGroup`s as an experimental plugin, because it can be a standalone component and isn't requiring many changes in the napari codebase itself
        - so adding it back later wouldn't be that hard
- [6611](https://github.com/napari/napari/pull/6611)
    - would be good to keep emissions in the data class rather than in mouse bindings
    - WM will update 6611 to just pass the selected data to the final `points._move` call
- npe2 needs a release
    - Grzegorz will try to push a tag and hopefully it releases
    - we need to add more people to the pypi project as currently only juan, talley and nick are on it



## 2024-01-10

- PRs for 0.4.19 [#6583](https://github.com/napari/napari/pull/6583), [#6542](https://github.com/napari/napari/issues/6542)
- Briefly going over [test](https://github.com/melonora/napari/pull/5) docstrings for axis labels 
- Idea from Grzegorz: change default colormap size to 64 so that mapping can be bitshift (by default) and be extremely fast

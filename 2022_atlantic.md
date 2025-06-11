## 2022-12-21
- Announcement: Take the short napari annual survey! https://bit.ly/napari-survey-c
    - The team uses the survey to capture big picture feedback, that is vital to ensuring napari serves the needs of the imaging community and making strategic changes.
Every 10th completed survey respondent will receive a $100 Amazon gift card.
Please share with all your napari-using colleagues and friends!

- Command search palette [here](https://github.com/napari/napari/issues/5430)
    - would love the team's feedback on scope for initial build, user flows, and UX mockups
    - looking for feedback from the community today
    - Lorenzo: possible to have a persistent command search line?
        - instead of a popup
        - ability to dock and move around a widget could be useful
        - chose popup to keep this unobstrusive
            - not modal
            - no auto-close so that it does persist in some way
        - open to changing this
        - dockable widget might make this too crowded when there is more information here in the future (tooltips, help strings may be long)
        - Lorenzo: even if this a popup by default, it could still be dockable
            - maybe similar to the popup for fine-grained contrast limits
    - Lorenzo: unsure if there's much advantage in multiple searches in parallel
        - Lisa: no intent to have two search windows open at once
    - Grzegorz: typically, command palette is mainly focused on keyboard usage
        - ideally want to be able to use it entirely from the keyboard
        - Lisa: escape key to close the window? yes
            - Grzegorz: ideally customizable as keyboard shortcut in settings
    - Joel: how did you create the user flows?
        - figma
    - Peter: placing the popup centered at the top feels right
        - similar to vscode, and the contrast limits controls
    - Lorenzo: maybe need a new general state for dockable widgets that allow them to be dismissed easily

- PR [#5432](https://github.com/napari/napari/pull/5432), initial work seen as a prerequisite for multicanvas view.

- :point_up: add new agenda items to this list! :point_up_2:

## 2022-12-07

- Peter: viewer theme PR [5143](https://github.com/napari/napari/pull/5143#issuecomment-1336213640)
  - decouple current viewer theme from settings, so changing viewer theme doesn't change settings
  - how to handle `system`? It's not a real theme, should the viewer 'remember' the setting was system or just the actual theme (light or dark)
  - consesus seems to be 1) make sure canvas is decoupled and 2) leave current behavior of viewer.theme reporting `system` for that case
- Lorenzo: Overlays 2.0 [4894](https://github.com/napari/napari/pull/4894)
    - ready for some feedback!
- Lorenzo: random test failures with Windows CI? (segmentation faults) anyone have any ideas?
    - Grzegorz: would be nice if someone with Windows could reproduce locally


## 2022-11-23

- Grzegorz: settings per environment [5333](https://github.com/napari/napari/pull/5333)
  - separate settings per environment so remove probability of collision of configuration between two env with different napari version and/or plugins set
  - user need to manually set every settings in each environment (like key bindings)
- Grzegorz: add Ruff to pre commmit [5275](https://github.com/napari/napari/pull/5275)
  - extremly fast python linter
  - we could also drop flake8 and speedup pre-commit actions. 
  - in really fast development so may require additional actions on version bump. 
- Grzegorz: add Pcln [5359](https://github.com/napari/napari/pull/5359) to pre commit
  - cleans unused import from codebase
- Kyle: early prototype of multiscale tiled rendering, [see](https://github.com/kephale/napari-multiscale-rendering-prototype)
    


## 2022-11-09

### Agenda

- Grzegorz: absolute imports
    - https://github.com/napari/napari/pull/5318
    - pre-commit hook
- Andy: release
    - planning to push the button shortly
- Andy: versioned settings
    - quick fix for main?
    - https://github.com/napari/napari/pull/5306
        - modify this to create a dev subdirectory to temp fix?
- Kyle: how evil would a BigDataViewer Layer be
    - BDV gives some functionality that current experimental oct/quadtree does not (e.g. non-orthogonal slicing)
    - would need JVM
    - would not see BDV window, but would use share memory with underlying libraries
- Jonas: napari-dataset
    - Current layergroups PR: https://github.com/napari/napari/pull/5132
        - Hasn't been touched in a while
        - Solves hierarchical structure and ability to control shared properties (e.g. opacity), also allows editing the tree
        - Does not solve extra stuff in napari-dataset: categories that span datasets
        - Need to fix qt segfault bug
        - Needs some work on vispy side, but should be somewhat straightforward
- Lisa: Async slicing UX design updates
    - [link here](https://drive.google.com/file/d/1Z4e1gjhFWutH1FtVd8g20dDRk4hR7Gi1/view?usp=sharing)
    - designs will be posted on GitHub shortly
    - consider using the existing warning/error dialogs instead of the right side of the status bar, if appropriate
- Wouter-Michiel/Lorenzo: multicanvas plans:
    - WIP hackmd document: https://hackmd.io/1hqVWWbFQmKRaUk8RL2bVw?view
    - first task may be to implement napari's current grid mode with vispy viewboxes, possibly with linked cameras sometimes
        - this would allow us to remove the layer's world2grid transform
        - and would enable functionality that the current grid mode does not affect


## 2022-10-26

### Agenda

- Jan-Hendrik Müller Napari-Cheat-Sheet: https://kolibril13.github.io/plywood-gallery-napari/
    - Slides for the presentation: https://kolibril13.github.io/pitch-napari/
    - available as interactive cheat sheet (jupyter notebook -> HTML), and as a VS code extension
    - how to incorporate this into the napari ecosystem?
        - for code snippets that are too small to be their own plugin, this can be useful to generate small code snippets
        - separate repository for this that people could contribute new snippets
            - allows for faster updates, not dependent on main napari or docs repo
    - could this be a napari plugin?
        - populate console with code snippet?
        - unsure if this is good idea
- Andy: release update...
    - Andy: will revert the commit that caused the circular import (but keep the tox.ini changes)
    - Andy: will test against latest pint branch and will update issue on that repo. Should accelerate release and fix napari upstream.
    - Andy: longer term wants to have a retrospective on the release process


## 2022-10-12

### Agenda

- Grzegorz: Remove uninstalled plugin from plugin manager [#5190](https://github.com/napari/napari/pull/5190) - looking for review
- Kyle: HDF5 support, [issue 4834](https://github.com/napari/napari/issues/4834)
- Kyle: Dynamic meshes/surfaces, [example image on zulip](https://napari.zulipchat.com/#narrow/stream/212875-general/topic/direct.20use.20of.20vispy.20in.20napari/near/303659374)
- Andy: release update

## 2022-09-28



### Agenda
- FYI: Participate in UX study on evaluating plugins with metrics!
    - Sign up [here](https://calendly.com/imaging_czi/plugin-metrics-study?utm_campaign=2022h2&utm_source=meeting) for a 45-60 minute session or [learn more](https://napari.zulipchat.com/#narrow/stream/212875-general/topic/Participate.20in.20UX.20study.20on.20evaluating.20plugins.20with.20metrics)
- Lorenzo: [`EventedModel` validation and inplace mutability](https://github.com/napari/napari/pull/5060): needs feedback and decision
- Lorenzo: [instanced rendering in vispy](https://github.com/vispy/vispy/pull/2378) works, but how to expose it in napari? (spoiler: nD rotations are *nasty*)
- Lorenzo: thoughts on the layergroup odyssey:
    - layergroups
        - qt segfault issues
        - decoupling from canvas (grid mode)
            - multicanvas
            - overlays
            - async work (remove slicing info from layer)
- Andy: v0.4.17 release update
    - zulip post: https://napari.zulipchat.com/#narrow/stream/215289-release/topic/0.2E4.2E17rc3/near/301121694
    - github release: https://github.com/napari/napari/releases/tag/v0.4.17rc3
- Peter: toggle theme is annoying, solution: 
- Grzegorz: settings: per-env, per-version, or global?

### Action items

At the end of the meeting:
- Copy the contents of this document to a new file in https://github.com/napari/meeting-notes
    - Final additions?
    - Release note highlights
- Clear out the Agenda from last meeting


### Side notes:

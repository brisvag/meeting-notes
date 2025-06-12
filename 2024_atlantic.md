## 2024-12-18

- [WM] #7209 iterative roi screenshot
    - PR is pretty much ready to go except for an issue in pytest with asserting certain values. Manual testing works but test fails.
- [WM] #7099 polyline drawing, good for merge?

## 2024-10-23

- discusion 

## 2024-10-09

- question from imagesc forum about bundle v. mamba install
    - add an explanation about how to setup a local napari to the docs
- bundle app
    - get more knowledge about certificate handling
    - get more information about porting plugins to conda-forge
    - need to get more people added to conda feedstocks for maintenance of key packages

- Grzegorz:
    - Speedup of triangulation for shapes: https://github.com/napari/napari/pull/7268

## 2024-09-25

- Grzegorz:
    - Speedup of triangulation for shapes: https://github.com/napari/napari/pull/7268
- Kyle:
    - Lasso for shapes in cellpose: https://github.com/MouseLand/cellpose-napari/pull/54

## 2024-09-11
- Grzegorz:
    - Reset contrast range for functional widget [5136](https://github.com/napari/napari/pull/5136)
    - Speedup shapes creation, point that sometimes is better separatelly treat edges [7256](https://github.com/napari/napari/pull/7256)
    - Speedup get status for shapes layer [7144](https://github.com/napari/napari/pull/7144)
    - Some decision about hanging units PR [6986](https://github.com/napari/napari/pull/6986), [6827](https://github.com/napari/napari/pull/6827)
- Isabela:
    - [Multi-canvas updates](https://github.com/napari/napari/issues/5348#issuecomment-2297739649)
    - I will be spending time on [contributable menus](https://napari.org/stable/naps/6-contributable-menus.html) so multi-canvas work is slowing down for me. It would be good to make sure it's in a steady place.
- Lorenzo:
    - nice ui log viewer [6900](https://github.com/napari/napari/pull/6900)

## 2024-07-17

- Isabela
    - [Multi-canvas updates](https://github.com/napari/napari/issues/5348#issuecomment-2232026861) on what adding a canvas takes and places grid mode could move to.

## 2024-07-03
- Isabela
    - [Multi-canvas](https://github.com/napari/napari/issues/5348#issuecomment-2195903763) update 
- Andy
    - Deprecated layer state: https://github.com/napari/napari/pull/6976
        - A little unsure about typing here, especially given layer-data-tuple is advertised as `dict` in napari and npe2 types module. `UserDict` is helpful but is strictly a `MutableMapping` that happens to implement the same interface as `dict`.
            - Decision: `UserDict` for implementation (for those benefits), but keep `dict` for typing (for simpler communication with user and no change). We can tell mypy to be quiet if it complains.
        - Will follow up with more changes to support deprecations other than renames (but maybe not in time for v0.5.0)
        - Should `__contains__` return True for deprecated keys? Consistent with `__getitem__`, but not `items`, `keys`, `__iter__`, etc.
            - Decision: `__contains__` should return True because we think it's a common pattern to check membership and we want to warn and maintain behavior.
    - Fix 3D display of multi-scale image: https://github.com/napari/napari/pull/6319
        - Already address by Lorenzo

## 2024-06-19
- Alessandro
    - another go at layer groups has started :sweat_smile: (in an [experimental plugin](https://github.com/brainglobe/napari-experimental))

## 2024-06-05
- wm
    - [#6730](https://github.com/napari/napari/pull/6730) screenshot functionality without margins. Agreement on API. Providing a screenshot without margins of exactly what is shown comes with edge cases. I would like a decision on merging as is with maybe small refactor of the private screenshot function.
- Isabela
    - [Multi-canvas](https://github.com/napari/napari/issues/5348#issuecomment-2150349638) update: use cases and pain points 
- Grzegorz https://czaki.github.io/napari_dashboard/ 

## 2024-05-08
- Sesan / WM: 
    - [#6730](https://github.com/napari/napari/pull/6730) screenshot functionality without margins.
    - [#6892](https://github.com/napari/napari/pull/6892) scaling scale bar based on canvas size.
- Isabela:
    - I'll be starting to work on [#5348](https://github.com/napari/napari/issues/5348): multi-canvas.

## 2024-04-24

- Wouter: multicanvas 
- Isabela: Updates on [layer groups](https://github.com/napari/napari/issues/6345#issuecomment-2075219839)!

## 2024-04-10

- Grzegorz: going over units [draft PR](https://github.com/napari/napari/pull/6780)

## 2024-03-27

- Isabela: Updates on [layer groups](https://github.com/napari/napari/issues/6345#issuecomment-2020901876)! Huzzah!

## 2024-03-13 
- Grzegorz: Drop python 3.8 and pyside 6 problems https://github.com/napari/napari/pull/6738
    - pyside6 action plan:
        - try to support 6.4.1 on linux on 3.9 (only issue (I hope) is flashing on macOS): https://github.com/napari/napari/issues/5644 which is fixed in 6.5)
        - Grzegorz will look at the viewer test failure with 6.4.3 and up: https://github.com/napari/napari/issues/5657
        - if that is solved, then the MRO issue with 6.5 (https://github.com/napari/napari/issues/5703) maybe fixed by https://github.com/napari/napari/pull/6062


- Discussion on painting/annotation in multiscale, as well as overall interface
    - would be nice if painting/annotation works via overlays and then is converted to the backend (shape or label, etc)
    - so that interactions are consistent
    - for multiscale labels "drawing" an overlay and then rasterizing to pixels after completion of painting

## 2024-02-28
- Grzegorz: [napari lite](https://github.com/napari/napari/issues/5940)
- Kyle: napari graph update


## 2024-02-14
- small turnout, just Kyle, Grzegorz, Peter
    - Grzegorz: make community calendar more prominant
    - Peter: https://github.com/napari/docs/issues/353
- Grzegorz [Allow to use precompiled backend for mapping](https://github.com/napari/napari/pull/6617)
- Grzegorz [Possible patch release](https://napari.zulipchat.com/#narrow/stream/215289-release/topic/0.2E4.2E20.2F0.2E4.2E19post1/near/421232795)

### Notes

Where do we put data for building examples & tests? 
- example with issue: https://napari.org/stable/gallery/surface_multi_texture.html#sphx-glr-gallery-surface-multi-texture-py
- Figshare requires a token (e.g. https://figshare.com/articles/media/PocilloporaDamicornisFreshSample/22348645/1).
- We could put this on Zenodo.
- We could also generate our own data.
- We could see if imagej.net wants to host it (IIRC those are university servers so they probably dont have to pay for bandwidth)


## 2024-01-31

- Isabela: [layer groups](https://github.com/napari/napari/issues/6345) update!
    - Small update of current status with showing some mockups. Current considerations are things like do we show a thumbnail for a group or not and do we show layers as indented or not (perhaps problematic with long layer names.)
    - [Kyle] did we think about layer grouping in the context of scene graphs in which we for example have a parent layer and then a segmentation labels layer would be counted as a child. 
    - [Isabela] currently not, but will pick it up. 
    - [WM] There are 2 ways of grouping where with scene graphs probably we could have an additional dropdown menu for coordinate spaces which is more the approach thinking of the scene graph, while there is also semantic grouping (certain layers together make up markers of a cell) which could be useful for multichannel views.
    - [Andy] There is also the editing viewpoint of adjusting certain properties of a group of layers at once. 
    - [Alessandro] note that we have been thinking about creating an experimental napari plugin to implement this. Haven't gotten really far yet. Mostly it is recycling existing code in the napari PRs.
    

## 2024-01-17

- Alessandro: what next for group layers?
- Isabela: would like to give a [layer groups](https://github.com/napari/napari/issues/6345) update.

- Peter small zarr demo if time


## 2024-01-03

- Alessandro interested in contributing cursor tooltips for napari layers.
    - discussed in an [imagesc forum post](https://forum.image.sc/t/napari-tooltip-for-cursor-when-moving-around-viewer/85142/13) - maybe should be converted into an issue?
    - [hacky implementation](https://github.com/brainglobe/brainrender-napari/pull/85/files) within BrainGlobe exists
        - use case in BrainGlobe is to display the name of the brain region over which the cursor is currently hovering
        - should work in dark and light theme (but currently doesn't because of background colour in QLabel qss?) and [sometimes the QLabel isn't deleted properly](https://github.com/brainglobe/brainrender-napari/issues/88).
        - currently uses private and soon to be deprecated Qt API of napari
    - guidance/thoughts on whether it is desirable to move this into napari and, if so, how to implement this would be appreciated.
        - what would the API look like for others to customise cursor tooltips?
```python
# in plugin code

my_layer.mouse_move_callback.append(self._on_mouse_move)

def _on_mouse_move(self, layer, event):
    tooltip = "a string possibly dependent on event.position"
    layer.set_cursor_tooltip(tooltip)
```


# arscene documentation

<!-- pages -->
- [API reference](api.md): every public function and type
<!-- /pages -->

## The stage

`ar_stage(&scene, camera)` makes the scene's background transparent and keeps what an AR
frame needs on the GPU. `ArStage_frame(&scene, frame, width, height)`, called in the
`use_ar_session` handler with the view's size in pixels:

- draws the camera picture (eskiu-lang/ar's YUV planes, through `<gpu/yuv>`) into a texture
  of the view's size, fitted as the session's projection assumes, and makes it the scene's
  `camera_background`;
- puts the camera node at the device's pose (under the node's parent, whatever it is) and
  gives it the session's projection (`scene_projection`, `near` 0.02 m and `far` 100 m);
- with `occlusion` on (the default) and a depth image in the frame (`ArOptions.depth` for
  the room, `ArOptions.people` for people, both together on LiDAR iPhones),
  uploads it in meters and makes it the scene's `occlusion_depth`, with the transform from the
  view to the picture, so virtual content behind a real surface is hidden;
- keeps the frame's light estimate in `stage.light`, to scale the scene's lights by.

## Placing

`ar_placer(&scene, content)` hides `content` (a node holding what to place) until it is
placed. Each frame, `ArPlacer_frame` draws the surfaces found (`plane_color`, at the scene's
root) and a ring where the middle of the view meets one, and says in `center_hit` and
`center_kind` whether it does and what the surface is (an `ArPlaneClass`). `ArPlacer_tap`
places the content where a tap meets a surface: at the hit, turned about the surface's normal
to face the user, at `scale`; it returns false when the tap meets nothing. After a placement
the surfaces hide (`hide_planes_when_placed`) and a floor under the content shows its shadows
on the real one (`shadows`; a light in the scene must cast shadows). `kinds`, a bit per
`ArPlaneClass` (`1 << ArPlaneTable`), keeps the placement to those surfaces;
`use_depth` lets a tap place on any real surface the depth image sees where no plane was
found (`ArFrame_depth_hit`); `ArPlacer_place` places at a pose of your own and `ArPlacer_reset` hides the content again.

## Markers

`ar_markers()`, then `ArMarkers_add(&scene, image, node)` for a node per image target
(`image` is its index in `ArOptions.images`): each frame, `ArMarkers_frame` moves the node to
its image's pose and shows it while the image is tracked (or last known, with
`keep_last_known`), and hides it otherwise. `ArMarkers_on_change` is told each image found and
lost.

## Tests

`tests/arscene.esk` runs with no device: frames built by hand and stand-ins for ar's native
side. The stage draws the picture behind an empty scene, places the camera at the device's
pose with the session's projection and passes the depth on; the placer draws the plane and
the ring, places on a tap at its scale, hides the planes and shows the shadow floor, and
refuses a surface of a kind not allowed; markers follow a found image and hide a lost one.
`python3 -I tools/gen_api.py` regenerates `docs/api.md`.

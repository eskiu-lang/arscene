# Changelog

All notable changes to this package are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-11-16

The first release. Needs Eskiu 0.15.1 or newer; brings ar 0.2.0, scene 0.2.0, gpu 0.7.0 and
ui 0.6.0.

### Added

- `ArStage`: each AR frame into a scene: the camera picture as the background, the device as
  the camera with the session's projection, depth occlusion, the light estimate.
- `ArPlacer`: tap to place (ViroARPlaneSelector): translucent planes, a ring at the middle of
  the view, placement facing the user at a scale, a shadow-catching floor, kinds of surface.
- `ArMarkers`: nodes on tracked images with found and lost events (ViroARImageMarker).
- `ArWorldMesh`: the LiDAR reconstruction as scene nodes, invisible occluders by default or
  shown in a color.
- `ArStage` passes the session's scene semantics to the scene, for semantic masks.
- Physics against the real world: `ArWorldMesh.physics` (a body per LiDAR piece) and
  `ArDepthCollider` / `ar_depth_triangles` (the depth image as a triangle body).
- `ArMarkers_add_object`: nodes on ARKit object targets (ViroARObjectMarker).

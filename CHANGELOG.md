# Changelog

All notable changes to this package are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added

- `ArStage`: each AR frame into a scene: the camera picture as the background, the device as
  the camera with the session's projection, depth occlusion, the light estimate.
- `ArPlacer`: tap to place (ViroARPlaneSelector): translucent planes, a ring at the middle of
  the view, placement facing the user at a scale, a shadow-catching floor, kinds of surface.
- `ArMarkers`: nodes on tracked images with found and lost events (ViroARImageMarker).

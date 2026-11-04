# Original S-FAST_LIO data flow

This document records the frozen `develop` baseline before the first refactor.

## Runtime flow

1. `livox_pcl_cbk` or `standard_pcl_cbk` receives LiDAR data, preprocesses it
   through `Preprocess`, and pushes the resulting cloud and timestamp into the
   shared LiDAR buffers.
2. `imu_cbk` receives IMU messages, applies the configured time offset and
   pushes them into the shared IMU buffer.
3. `sync_packages` waits until IMU data covers the end time of the next LiDAR
   scan, then creates a `MeasureGroup`.
4. `ImuProcess::IMU_init` initializes gravity, biases, and the initial state.
5. `ImuProcess::UndistortPcl` propagates a temporary IMU state and deskews the
   LiDAR scan.
6. `esekfom::esekf::update_iterated_dyn_share` queries `ikd-Tree`, builds the
   point-to-plane residual/Jacobian, and performs the iterated update.
7. `lasermap_fov_segment` maintains the local map window.
8. `map_incremental` transforms filtered points to the world frame and inserts
   them into `ikd-Tree`.
9. The main loop publishes odometry, path, point clouds, and timing statistics.

## Baseline scope

- Normal mapping entry point: `src/laserMapping.cpp`.
- Alternate entry point: `src/laserMapping_re.cpp`.
- Filter implementation: `include/esekfom.hpp` and `include/use-ikfom.hpp`.
- IMU initialization and deskewing: `src/IMU_Processing.hpp`.
- Point preprocessing: `src/preprocess.cpp` and `src/preprocess.h`.
- Local map: `include/ikd-Tree/`.

The first refactor must preserve the normal mapping behavior and the current
point preprocessing and local-map backend until the replacement modules have
independent tests.

# S-FAST_LIO develop baseline

## Repository

- Branch: `develop`
- Remote: `origin/develop`
- Baseline commit: `d2cad81` (`first commit`)
- Working tree: clean when this baseline was recorded

## Build and runtime record

Fill in the following fields using the fixed local build and rosbag before
algorithm changes are merged:

```text
Compiler:
ROS distribution:
Eigen version:
PCL version:
Sophus version/commit:
Build command:
Rosbag:
Configuration YAML:
Configuration SHA:
```

Record at least:

- IMU initialization completion time
- Per-frame pose and 24-state values
- 24x24 covariance
- Number of valid plane matches
- Output cloud/map artifacts
- Mean and P95 processing time
- Peak memory, if available

All later comparisons must include the Git SHA, rosbag, and configuration SHA.

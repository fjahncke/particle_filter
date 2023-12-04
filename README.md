# Particle Filter Localization

This code implements the MCL algorithm for the RACECAR. 

[![YouTube Demo](./media/thumb.jpg)](https://www.youtube.com/watch?v=-c_0hSjgLYw)

For high efficiency in Python, it uses Numpy arrays and [RangeLibc](https://github.com/f1tenth/range_libc) for fast 2D ray casting.

# Installation

To run this, you need to ensure that both the map_server ROS package, and the python wrappers for RangeLibc are installed.

For the map server:
```
cd f1tenth_ws/src
git clone https://github.com/fjahncke/particle_filter.git
cd particle_filter
sudo apt-get update
rosdep install -r --from-paths src --ignore-src --rosdistro foxy -y
```

Install range_lib:

```
See here: https://github.com/fjahncke/range_libc
```

# Usage

The majority of parameters you might want to tweak are in the launch/localize.launch file. You may have to modify the "odometry_topic" or "scan_topic" parameters to match your environment.

```
1. Build and source
2. ros2 launch particle_filter localize_launch.py
```

! The map only works with PNG Images. Simply use an online converter to convert your maps from PGM to PNG. !

Once the particle filter is running, you can visualize the map and other particle filter visualization message in RViz. Use the "2D Pose Estimate" tool from the RViz toolbar to initialize the particle locations.

Add the general /map topic and the particle filter odometry.


See [launch/localize.launch](/particle_filter/launch/localize.launch) for docs on available parameters and arguments.

The "range_method" parameter determines which RangeLibc ray casting method to use. The default is cddt because it is fast and has a low initialization time. The fastest option on the CPU is "glt" but it has a slow startup. The fastest version if you have can compile RangeLibc with CUDA enabled is "rmgpu". See this performance comparison chart:

![Range Method Performance Comparison](./media/comparison.png)

# Docs

This code is the staff solution to the lab guide found in the [/docs](/particle_filter/docs) folder. A mathematical derivation of MCL is available in that guide.

There is also documentation on RangeLibc in the [/docs](/particle_filter/docs) folder.

The code itself also contains comments describing purpose of each method.

# Cite

This library accompanies the following [publication](http://arxiv.org/abs/1705.01167).

    @article{walsh17,
        author = {Corey Walsh and 
                  Sertac Karaman},
        title  = {CDDT: Fast Approximate 2D Ray Casting for Accelerated Localization},
        volume = {abs/1705.01167},
        url    = {http://arxiv.org/abs/1705.01167},
        year   = {2017}}

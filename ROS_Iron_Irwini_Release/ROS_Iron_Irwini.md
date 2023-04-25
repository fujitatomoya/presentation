---
marp: true
theme: default
header: "__ROS Iron Irwini Release__"
footer: "[fujitatomoya@github](https://github.com/fujitatomoya)"
---

# ROS Iron Irwini - What's new -

<!---
Comment Here
--->

---

# Supported Platform

- Tier 1 platforms
  - Ubuntu 22.04 (amd64/aarch64)
  - Windows 10 (VS 2019) (amd64)
- Tier 2 platforms
  - RHEL 9 (amd64)
- Tier 3 platforms
  - macOS (amd64)
  - Debian Bullseye (amd64)

<!---
Comment Here
--->

---

# New Features

<!---
Comment Here
--->

---

# Service Introspection

This allows users to see the metadata and activities associated with the service, such as request, response. Optionally, the contents of the client/server requests/responses can also be introspected.
All of the information is published on a hidden topic generated from the name of the service.
So if the service is called `/myservice`, then the information will be published on `/myservice/_service_event`.

<!---
Comment Here
--->

---

- This is `opt-in` that can be enabled via `configure_introspection` on service object.
- See more details [REP 2012](https://github.com/ros-infrastructure/rep/pull/360)
- See [Client Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/services/introspection_client.cpp) and [Service Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/services/introspection_service.cpp)
- Additional features could leverage this feature next?
  - `ros2 service echo`
  - Playback recorded service via rosbag2
  - Introspection for ROS actions (rosbag2)

<!---
Comment Here
--->

---

# `Pre` and `Post` parameter callback support

`on_set` parameter callback has been supported for many releases, but it does not allow the user application flexible parameter management with callback. The new parameter callbacks support modify the parameter list incoming, modify the state if necessary.

<!---
Comment Here
--->

---

- `pre` set parameter callback, which can modify the list of parameters based on arbitrary criteria.
- `set` parameter callback, which cannot modify the list and should only accept or reject the parameters based on their type and value (already supported).
- `post` set parameter callback, which can make state changes based on parameters and is only called if the previous two callbacks are successful.
- See [C++ Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/parameters/set_parameters_callback.cpp) and [Python Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_py/demo_nodes_py/parameters/set_parameters_callback.py)

---

# Matched Events

Matched events can be generated when any publisher and subscription establishes or drops the connection between them. Users can provide each publisher and subscription with callback functions that are triggered by matched events and handle them in a way they see fit, similar to how messages received on a topic are handled.

- See more details for [matched event design](https://github.com/ros2/rmw/issues/330)
- See [C++ Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/events/matched_event_detect.cpp) and [Python Demo](https://github.com/ros2/demos/blob/iron/demo_nodes_py/demo_nodes_py/events/matched_event_detect.py)

---

![bg 90%](https://user-images.githubusercontent.com/43395114/189972176-536b8108-6b39-413d-aaca-6e4475e1ca5e.png)

<!---
Comment Here
--->

---

# External configuration services of logger

It is now possible to configure node logger levels remotely via a service.

When the enable_logger_service option is enabled during node creation, the `set_logger_levels` and `get_logger_levels` services will be available.

Be advised that the `enable_logger_service` option is disabled by default, so the user needs to enable this option on node creation.

- See more details for [External logging level configuration](https://github.com/ros2/ros2/issues/1355)

<!---
Comment Here
--->

---

![bg 80%](https://user-images.githubusercontent.com/43395114/205733118-bb63f173-4822-4c23-8239-f63fb07d36b8.png)

<!---
Comment Here
--->

---

# rclcpp

- Addition of a new paradigm for handling `Node` and `LifecycleNode` with `NodeInterfaces`
- default number of threads for `MultiThreadedExecutor`.
  - If the user doesn’t specify otherwise, the default number of threads for the multi-threaded executor will be set to the number of CPUs on the machine. If the underlying OS doesn’t support getting this information, it will be set to 2.

<!---
Comment Here
--->

---

# rclpy

- Ability to wait for another node to join the graph (`node.wait_for_node("fqdn")`)
- `AsyncParameterClient` supported
- Subscription callbacks can now optionally get the message info
- Optional argument that hides assertions for messages class (performance improvement)
- default number of threads for `MultiThreadedExecutor`. (see rclcpp)

<!---
Comment Here
--->

---

# ros2param

- `--timeout` option to `ros2 param` when waiting node.
- `ros2 param dump` option `--output-dir` and `--print` has been removed.

<!---
Comment Here
--->

---

# ros2topic

- `now` keyword for `builtin_interfaces.msg.Time` that automatically generate the time.
- `ros2 topic pub` with `--max-wait-time` to wait the subscription.
- `ros2 topic echo` with `--timeout` to wait the publication.
- `--lost-messages` option `ros2 topic echo` has been removed.
- `ros2 topic hz/bw/pub` now respect `use_sim_time`.

<!---
Comment Here
--->

---

# rosbag2

- default bag file type to `mcap`
  - default bag file type has been changed into `mcap` from `sqlite3`.
  - `mcap` file format has many of the missing features from the sqlite3 file format, and should also be more performant.
  - The old `sqlite3` file format is still available and can be selected by the user for writing if desired.

<!---
Comment Here
--->

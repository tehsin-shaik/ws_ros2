# ROS 2 Publisher–Subscriber Workspace

A Python ROS 2 workspace demonstrating communication between a publisher and a subscriber. The `simple_pubsub` package sends numbered greetings over a ROS topic and logs them as they arrive.

> **Branch:** The implementation is on the [`tehsin` branch](https://github.com/tehsin-shaik/ws_ros2/tree/tehsin). 

## Overview

This example introduces core ROS 2 concepts through two small Python nodes:

- Creating nodes with `rclpy`
- Publishing and subscribing to `std_msgs/msg/String` messages.
- Using a timer to publish once per second.
- Handling incoming messages with a subscription callback.
- Building an `ament_python` package with `colcon`.

No robot hardware or simulator is required for this example.

## Nodes and Topic

| Component | Name | Behavior |
| --- | --- | --- |
| Publisher node | `talker` | Publishes `Hello RoboCup students #N` once per second, starting at `#1`. |
| Subscriber node | `listener` | Receives each message and logs it with an `I heard:` prefix. |
| Topic | `/student_chatter` | Carries messages of type `std_msgs/msg/String`. |

Both nodes configure a queue depth of `10`.

## Repository Contents

| Path | Purpose |
| --- | --- |
| `src/simple_pubsub/simple_pubsub/talker.py` | Publisher node and timer callback. |
| `src/simple_pubsub/simple_pubsub/listener.py` | Subscriber node and message callback. |
| `src/simple_pubsub/package.xml` | ROS package metadata and dependencies. |
| `src/simple_pubsub/setup.py` | Python package configuration and `talker` / `listener` entry points. |
| `src/simple_pubsub/setup.cfg` | Executable installation paths. |
| `src/simple_pubsub/test/` | Copyright, formatting, and docstring checks. |

## Requirements

- A working ROS 2 installation with Python 3 support.
- `colcon` with ROS package support.
- `rosdep`, initialized for your system.
- Git.

The package depends on `rclpy` and `std_msgs`. The repository does not specify a tested ROS 2 distribution. The commands below use Bash on Linux and assume you have already installed ROS 2 and its development tools.

## Getting Started

### 1. Activate ROS 2

Source your ROS 2 installation's setup script. For a standard installation under `/opt/ros`, replace `YOUR_DISTRO` with your installed distribution name:

```bash
source /opt/ros/YOUR_DISTRO/setup.bash
```

Use the appropriate setup path if you installed ROS 2 elsewhere.

### 2. Clone the implementation branch

```bash
git clone --branch tehsin https://github.com/tehsin-shaik/ws_ros2.git
cd ws_ros2
```

### 3. Install package dependencies

With ROS 2 sourced and `rosdep` initialized:

```bash
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```

### 4. Build the package

Run from the `ws_ros2` directory:

```bash
colcon build --symlink-install --packages-select simple_pubsub
```

The build uses the standard [colcon build options](https://colcon.readthedocs.io/en/released/reference/verb/build.html) and [package selection workflow](https://colcon.readthedocs.io/en/released/user/how-to.html#build-only-a-single-package-or-selected-packages).

## Run the Example

Open two new terminals. In each terminal, activate the same ROS 2 installation and navigate to the cloned `ws_ros2` directory before running the following commands.

**Terminal 1 — subscriber**

```bash
source install/setup.bash
ros2 run simple_pubsub listener
```

**Terminal 2 — publisher**

```bash
source install/setup.bash
ros2 run simple_pubsub talker
```

The publisher's log messages should include:

```text
Publishing: "Hello RoboCup students #1"
Publishing: "Hello RoboCup students #2"
```

The subscriber's log messages should include:

```text
I heard: "Hello RoboCup students #1"
I heard: "Hello RoboCup students #2"
```

ROS 2 also adds log metadata such as timestamps and node names. Press `Ctrl+C` in each terminal to stop the nodes.

## Inspect the Running System

In another terminal with the same ROS 2 environment activated, use:

```bash
ros2 node list
ros2 topic list
ros2 topic info /student_chatter
ros2 topic echo /student_chatter
```

## Troubleshooting

- **`ros2` is not found:** Source your ROS 2 installation's setup script.
- **`simple_pubsub` is not found:** Confirm you cloned the `tehsin` branch, built the package successfully, and sourced `install/setup.bash` in the current terminal.
- **The listener receives nothing:** Check that both nodes are running with the same ROS 2 environment and `ROS_DOMAIN_ID`.

## Acknowledgments

This repository is a fork of [`longlinht/ws_ros2`](https://github.com/longlinht/ws_ros2). The publisher and subscriber implementation documented here is available on this fork's `tehsin` branch.

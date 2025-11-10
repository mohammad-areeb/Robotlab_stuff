# Turtlebot Desktop Development Container 

This repository contains a ready-to-use **Docker-based ROS 2 Humble** development environment for working with **TurtleBot3**. It includes Visual Studio Code support, useful extensions, and common ROS 2 packages preinstalled — making it easy to get started with TurtleBot simulation and development on any Linux machine.

---

## 📦 Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

---

## 🔧 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/Cobot-Maker-Space/turtlebot-desktop-container.git
cd turtlebot-desktop-container/src
```

---

### 2. Open in VS Code

1. Launch VS Code.
2. Click on Open in Folder option and then open the directory.
3. Open the `src/` directory of this repo.
4. You should see a `.devcontainer/` folder in the file tree.

---

### 3. Reopen in Dev Container

Open the PC's terminal and run:

```bash
xhost +local:docker
```
Inside VS Code:
  - Press `Ctrl+Shift+P`
  - Type and select: `Dev Containers: Reopen in Container`
  - VS Code will now build and launch your ROS 2 container

---

### 4. Test the Setup

Inside the container terminal:

```bash
source /opt/ros/humble/setup.bash
ros2 topic list
```

If ROS 2 is installed correctly, you’ll see an empty or populated list depending on what's running.

### 5. Connection with the Turtlebot (on Wi-Fi)

Inside the container, a variety of environment are already set up through devcontainer.json file which would be mathcing the turtlebot env variables, i.e., `ROS_DOMAIN_ID=30` , `ROS_LOCALHOST_ONLY=0` and `TURTLEBOT3_MODEL=waffle_pi`.

Now boot the turtlebot up and make sure its on the same network. Launch the bringup file on it, now if you run the topic list comamnd, you can see all the available topics that are running.

```bash
ros2 topic list
```

If the turtlebot is up, and you still can't view the topics try the above command after running the following commands:

```bash
ros2 daemon stop
ros2 daemon start
```
<!--
### 6. Conenction with the Turtlebot (on Wired Connection)

Run the following command in your container terminal:

```bash
fastdds discovery --server-id 0
```

Now open another terminal inside the container, and check the IP of the machine:

```bash
ifconfig
```

Save the IP and now run the following commands:

```bash
export ROS_DISCOVERY_SERVER=IP_ADDRESS:11811
export ROS_SUPER_CLIENT=TRUE
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp    # or cyclone on both, must match
```

Now run:

```bash
ros2 daemon stop

ros2 daemon start
```


The above coommands would basically creater a server and then we would make our terminals make clients susbscribing to that server.

We would run the above commands on the Turtlebot terminal too:

```bash
export ROS_DISCOVERY_SERVER=IP_ADDRESS:11811
export ROS_SUPER_CLIENT=TRUE
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp    # or cyclone on both, must match
```

Now run:

```bash
ros2 daemon stop

ros2 daemon start
```

And now launch the bringup file on it, you can see all the available topics that are running.

```bash
ros2 topic list
```
-->
### 6. After the Initial Tests

Once the container is set up and you want to make changes and create packages which you want to persist even after the container is closed. 

  - Press `Ctrl+Shift+P`
  - Type and select: `Dev Containers: Reopen folder locally`

Open the `devcontainer.json` and comment out the last line:

```bash
"postCreateCommand": "rm -rf build/* install/* log/* src/* && cd src/ && git clone -b humble https://github.com/ROBOTIS-GIT/DynamixelSDK.git && git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3.git && git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git && git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git && cd .. && colcon build --symlink-install"
```

And uncomment the line on top of it:

```bash
"postCreateCommand": "colcon build --symlink-install"
```

This step is crucial for making new custom packages and for reducing the time taken by the container to boot up.


## 🛠 Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| `Docker permission denied` | Make sure your user is added to the `docker` group: <br> `sudo usermod -aG docker $USER` <br> Then logout your user and then log back in and check. |
| `Cannot access /dev/video0` | Add your user to the `video` group: <br> `sudo usermod -aG video $USER` |
| When launching Gazebo simulations, if it takes too much time and exits at `Spawn service failed. Exiting.` | Do not press `Ctrl + C` Let it fail completely and cleanly and then close it and run it again. |
<!--| `No ROS 2 topics across devices` | Ensure matching `ROS_DOMAIN_ID`, set `ROS_LOCALHOST_ONLY=0`, and use same `RMW_IMPLEMENTATION`. And try to repeat the 6th Step in case of wired setup. | -->
---

## 💡 Notes

- Default user inside container is `team-beta` (non-root).
- Workspace is mounted to `/home/ros2_ws/src`.
- Includes support for Gazebo, SLAM, Navigation2, Teleop, Cartographer, and more.
- VS Code extensions preinstalled for ROS, C++, Python, and Git.

---


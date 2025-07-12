ROS Message Sharing across Computers
====================================

Copy the 2 system start scripts 'system_with_domain_bridge.sh' and 'system_with_route_planner_and_domain_bridge.sh' `in this repository <https://github.com/jizhang-cmu/gadgets/tree/jazzy/ros_message_sharing_across_computers>`_ to the home folder on the vehicle NUC computer. In a terminal, go to the home folder and use the command lines below to make the scripts executable. These 2 scripts start the system, without RVIZ, in the base autonomy mode and route planner mode together with the domain bridge. The domain bridge shares messages between domain 0 (vehicle NUC computer, default) and domain 1 (add-on AI computer). If more topics are to be shared, update the `'~/autonomy_stack_mecanum_wheel_platform/src/utilities/domain_bridge/config/domain_bridge.yaml' <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform/blob/jazzy/src/utilities/domain_bridge/config/domain_bridge.yaml>`_ file on the vehicle NUC computer or replace it with the one `in this repository <https://github.com/jizhang-cmu/gadgets/tree/jazzy/ros_message_sharing_across_computers>`_.

.. code-block:: XML

    chmod 777 system_with_domain_bridge.sh
    chmod 777 system_with_route_planner_and_domain_bridge.sh

Copy the 'data_view' folder `in this repository <https://github.com/jizhang-cmu/gadgets/tree/jazzy/ros_message_sharing_across_computers>`_ to the add-on AI computer. Follow the readme inside the folder to compile ROS workspace and set up desktop buttons. There are 2 desktop buttons, one for the base autonomy mode and the other for the route planner mode. Set them up on the desktop. Follow the `Installing Add-on Computer <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform/tree/jazzy?tab=readme-ov-file#installing-add-on-computer>`_ section in the GitHub repository readme to set the add-on AI computer to use manual IP at 10.1.1.101 (recommended, while the vehicle NUC computer is at 10.1.1.100) and set up chrony to synchronize the vehicle NUC computer. Chrony is already set up on the vehicle NUC computer and works as the time server at 10.1.1.100. Connect the vehicle NUC computer and add-on AI computer with an Ethernet cable (via USB to Ethernet adapter). Make sure the 2 computers are on the same subnet and can pin each other. 

At this point, you may ssh from one computer to the other computer to start the system. Use ``ssh all@10.1.1.100`` with password 'all' if ssh'ing from the add-on AI computer to the vehicle NUC computer. Turn off Wi-Fi on both computers. First, start system on the vehicle NUC computer using 1 out of the 2 system start scripts (command lines below), in base autonomy mode or route planner mode. Then, double-click a desktop button on the add-on AI computer to start RVIZ. Please use the desktop button in the same mode as the system start script on the vehicle NUC computer. Now, you should see data displayed in RVIZ and you can navigate the vehicle around. You can also turn on Wi-Fi on the add-on AI computer if needed.

.. code-block:: XML

    ~/system_with_domain_bridge.sh
    ~/system_with_route_planner_and_domain_bridge.sh

If cameras is install on the vehicle, users can connect the cameras to the add-on AI computer. Here, we use the Ricoh Theta Z1 camera as an example. Clone `the 360_camera repository <https://github.com/jizhang-cmu/360_camera>`_ to the add-on AI computer. Follow the readme inside the folder to install dependencies, compile ROS workspace, and set up the desktop button. With the camera power on and in 'live' mode, double-click the desktop button to start the camera driver. To calibrate the image latency, make sure the '/imu/data' topic is contained in the 'domain_bridge.yaml' file for the add-on AI computer to receive the messages. Follow the readme to back-and-forth rotate the vehicle to calibrate the latency. Also, calibrate the extrinsics to align the camera frame with the lidar frame.

The add-on AI computer has domain ID 1 across the computer. If running other custom nodes, set ``export ROS_DOMAIN_ID=1`` in all terminals before launching the node. Alternatively, add it to the '~/.bashrc' file. If using startup scripts to launch the nodes, add the line to all startup scripts at the top.

You may unplug the HDMI cable and USB power cable from the vehicle NUC computer and plug them into the add-on AI computer to use it from the control station. If warnings or errors show up in the vehicle NUC computer terminal continuously, reboot both computers. Likely, something isn't killed and still subscribes to the topics on either computer. Also, if sharing a large amount of data through ROS messages, e.g. sharing high-resolution images, use the command lines below to increase message buffer size on both computers. Then, use ``sysctl net.core.rmem_max`` and ``sysctl net.core.wmem_max`` to check the buffer size. Sharing a large amount of data between computers is not recommended. If images have to be shared, please compress the images with 'image_transport' first. Note that the Ricoh Theta Z1 camera driver can publish compressed images alongside the raw images.

.. code-block:: XML

    sudo sysctl -w net.core.rmem_max=67108864 net.core.rmem_default=67108864
    sudo sysctl -w net.core.wmem_max=67108864 net.core.wmem_default=67108864

When shutting down or rebooting, you can use the command lines below to shutdown or reboot one computer ssh'ed from the other computer.

.. code-block:: XML

    sudo shutdown now
    sudo reboot now

**Notes for message sharing between different Ubuntu versions, e.g. between Ubuntu 22.04 and 24.04:** ROS topic dyanmc connection is fragile. Connections should be established all at once when the system starts on the vehicle NUC computers. Please launch all nodes on the add-on AI computer (at domain ID 1) first, then launch the system on the vehicle NUC computer (at domain ID 0, default). When the system is running, keep the connections consistent on the add-on AI computer and avoid stopping and launching nodes.



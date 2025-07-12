Ubuntu System Setup
===================

This section contains information to set up Ubuntu system and the autonomy stack on the vehicle NUC computer from scratch, using Ubuntu 24.04 as an example. Download the latest Ubuntu 24.04 installation image, namely 'ubuntu-24.04.x-desktop-amd64.iso' `from this page <https://releases.ubuntu.com/noble>`_. Find a USB drive and an Ubuntu computer (can use the vehicle NUC computer). Go to the 'App Center' on the Ubuntu computer and install 'Startup Disk Creator'. Launch the app, select the installation image and the USB drive, and 'Make Startup Disk'.

Power off the vehicle NUC computer, plug in the USB drive, then power on the computer while pressing F10 to enter boot menu. Select the USB drive and follow the step-by-step instructions to install Ubuntu 24.04. Erase the original Ubuntu system and use the entire SSD for the new Ubuntu installation. Set computer name to 'sensorpod', and both username and password to 'all'.

After the installation, connect the vehicle NUC computer to WiFi and update the Ubuntu system, then add username to dialout group and reboot. Note that if the computer username is different from 'all', change the username accordingly in the command lines below.

.. code-block:: XML

    sudo apt update
    sudo apt upgrade
    sudo adduser all dialout
    sudo reboot now

Go to 'Settings->Network' and configure the wired network connected to the Mid360 lidar to 'Manual' settings, IP at '192.168.1.5', netmask at '255.255.255.0', and gateway at '192.168.1.1'. Go to 'Settings->Power' and choose 'Performance', set 'Screen Blank' to 'Never', 'Automatic Suspend' to 'Off', and 'Power Button Behavior' to 'Power Off'.

Now, install ROS2 Jazzy following `the instructions on this page <https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html>`_. After the installation, add the ROS installation path to the '~/.bashrc' file and source it in terminal to engage the installation.

.. code-block:: XML

    echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
    source ~/.bashrc

Follow the `System Setup <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform?tab=readme-ov-file#system-setup>`_ section in the GitHub repository readme to set up the autonomy stack. Go through the 5 steps from '1) All Dependencies' to '5) Full Repository'. Make sure to configure the Mid360 lidar driver to the specific lidar. In the `'~/autonomy_stack_mecanum_wheel_platform/src/utilities/livox_ros_driver2/config/MID360_config.json' <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform/blob/jazzy/src/utilities/livox_ros_driver2/config/MID360_config.json>`_ file, under the ``lidar_configs`` settings, set the lidar IP to 192.168.1.1xx, where xx are the last two digits of the lidar serial number. You can find it on a sticker under a QR code on the lidar. Pin the lidar to double-check its IP. 

.. code-block:: XML

    ping 192.168.1.1xx

After setting up the autonomy stack, you may follow the `System Usage <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform#system-usage>`_ section to launch the base autonomy system, the system with route planner, and the system with exploration planner. Use RVIZ and the joystick controller to navigate the vehicle around. Make sure to unplug and re-plug in the joystick controller dongle once after powering on the vehicle NUC computer and powering on the joystick controller.

Copy the icon image to the home folder and 3 desktop buttons onto the desktop. On the desktop, right-click each of the 3 desktop buttons and 'Allow Launching' to enable them. Now, you may double-click any of the desktop buttons to launch the system in the 3 modes. Note that if the computer username is different from 'all' or the autonomy stack is not in the default home folder, open the 3 desktop buttons in a text editor and modify the links in them. Each desktop button contains 2 links, one to the system startup script and the other to the icon image.

.. code-block:: XML

   cd ~/autonomy_stack_mecanum_wheel_platform
   cp ./desktop_buttons/start.png ~/
   cp ./desktop_buttons/*.desktop ~/Desktop/

Finally, install a couple more packages. Copy the provided chrony configuration file into the system and restart chrony. This creates a time server on the vehicle NUC computer at IP 10.1.1.100. When connecting to the add-on AI computer, configure the wired network on the vehicle NUC computer connecting to the add-on AI computer to 'Manual' settings, IP at '10.1.1.100', netmask at '255.255.255.0', and gateway at '10.1.1.1'. Now, the vehicle NUC computer can be connected to the add-on AI computer.

.. code-block:: XML

   sudo apt install net-tools openssh-server chrony
   cd ~/autonomy_stack_mecanum_wheel_platform
   sudo cp ./chrony_conf/chrony.conf /etc/chrony
   sudo systemctl restart chrony.service

To update the system, connect the vehicle NUC computer to Internet over WiFi or Ethernet (via USB to Ethernet adapter) and run command lines below. Then, reboot the computer.

.. code-block:: XML

    sudo apt update
    sudo apt upgrade
    cd ~/autonomy_stack_mecanum_wheel_platform
    git pull
    colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release


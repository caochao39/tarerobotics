Quick Start Guide
=================

Opening Box
-----------

The box contains the Mecanum wheel vehicle, battery charger, joystick controller, wireless HDMI receiver, TX60 split cable, TX60 to 5.5mmx2.5mm DC cable, USB-A to Ethernet adapter, USB-C to USB-A adapter, Ethernet cable, 2x 5v chargers, 2x USB-C cables, dummy HDMI plug, and 2x bungee cords. **Please make sure to charge the vehicle battery fully.**

|pic1| |pic2|

.. |pic1| image:: ./images/image42.jpg
   :width: 33.5% 

.. |pic2| image:: ./images/image43.jpg
   :width: 55%

Installing Antennas and Dongle
------------------------------

The antennas of the wireless HDMI sender are removed for shipping, please open the rear panel of the vehicle and install the antennas. Then, use zip ties to secure the antennas at a fixed angle. Also, plug in the joystick controller dongle to the USB hub on the vehicle. The joystick controller dongle, antennas, and zip ties are included in the box.

.. image:: ./images/image36.jpg
    :width: 70%

Setting up Control Station
--------------------------

Please prepare a monitor with HDMI cable and a set of wireless keyboard and mouse. Plug in the wireless keyboard and mouse dongle to the USB hub on the vehicle. Set up a control station by connecting the wireless HDMI receiver to the monitor with the HDMI cable and powering it on with the 5v charger and USB-C cable. Power on the vehicle by turning the switch clockwise all the way to the end and voltage is displayed on the switch. Wait for a minute and the onboard NUC i7 computer desktop should show up on the monitor.

.. image:: ./images/image37.jpg
    :width: 80%

The PS3 controller dongle may not be recognized correctly after the vehicle is powered on. **Please unplug and re-plug in the PS3 controller dongle once after powering on the vehicle for it to be recognized correctly. Once recognized correctly, the top 2 and bottom 2 status LEDs on the PS3 controller should be on (press center button on the joystick controller to turn it on).**

.. image:: ./images/image38.jpg
    :width: 45%

Launching System
----------------

From the control station, use the 3 desktop buttons to launch the system. Double-click the button named ‘start_system.desktop’ to launch the base autonomy system. Then, use a combination of the 'Waypoint' button and control panel in RVIZ and the joystick controller to operate the system **(make sure the joystick controller is powered on)**. Please follow instructions in the Switching Mode section below to operate the system in *smart joystick mode*, *waypoint mode*, and *manual mode*. To launch the system with route planner, use the button named ‘start_system_with_route_planner.desktop’ and send a goalpoint with the 'Goalpoint' button in RVIZ. To launch the system with exploration planner, use the button named ‘start_system_with_exploration_planner.desktop’ and click the 'Resume Navigation to Goal' button in RVIZ to start the exploration.

Launch base autonomy system:

.. image:: ./images/image39.jpg
    :width: 75%

Launch system with route planner:

.. image:: ./images/image40.jpg
    :width: 75%

Launch system with exploration planner:

.. image:: ./images/image41.jpg
    :width: 75%



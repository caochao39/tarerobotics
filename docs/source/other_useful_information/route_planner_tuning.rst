Route Planner Tuning
====================

Route planner configurations are in the `'autonomy_stack_mecanum_wheel_platform/src/route_planner/far_planner/config' <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform/tree/jazzy/src/route_planner/far_planner/config>`_ folder. The default configuration file is 'indoor.yaml'. To switch the configuration file, go to the `'autonomy_stack_mecanum_wheel_platform/src/base_autonomy/vehicle_simulator/launch' <https://github.com/jizhang-cmu/autonomy_stack_mecanum_wheel_platform/tree/jazzy/src/base_autonomy/vehicle_simulator/launch>`_ folder and find ``route_planner_config`` in the 'system_unity_with_route_planner.launch', 'system_real_robot_with_route_planner.launch', and 'system_bagfile_with_route_planner.launch' files, change it from 'indoor' to 'outdoor'.

If the environment contains dynamic obstacles, make sure ``is_static_env = false`` and can reduce ``util/dynamic_obs_dacay_time : 2.0 (s)`` to 1.0 or 0.5. 

If the visibility graph update is slow, try to reduce ``sensor_range : 15 (m)`` to 10 and ``terrain_range : 7.5`` to 5.0.

If the route planner tries to guide the vehicle through narrow openings that the vehicle cannot pass, try to adjust ``robot_dim : 0.5 (m)`` to 0.7 or 0.8, ``util/obs_inflate_size : 1`` to 2 or 3, and ``c_detector/resize_ratio : 3.0`` to 2.5 or 2.0. If the route planner is reluctant to enter large enough openings, adjust the parameters in the other direction.

If the visibility graph is messed up in cluttered environments, try to adjust ``graph/connect_votes_size : 10`` to 7 or 5, ``graph/clear_dumper_thred :4`` to 7 or 9, and ``graph/node_finalize_thred : 6`` to 3 or 2.

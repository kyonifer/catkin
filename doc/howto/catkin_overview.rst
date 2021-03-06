.. _catkin_overview_1:

Catkin configuration overview
-----------------------------

ROS and other packages may be configured and built using catkin.
Every catkin package must include ``package.xml`` and
``CMakeLists.txt`` files in its top-level directory.

package.xml
:::::::::::

Your package must contain an XML file named package.xml_, as specified
by REP-0127_.  These components are all required::

  <package>
    <name>your_package</name>
    <version>1.2.4</version>
    <description>
      This package adds extra features to rosawesome.
    </description>
    <maintainer email="you@example.com">Your Name</maintainer>
    <license>BSD</license>
    <buildtool_depend>catkin</buildtool_depend>
  </package>

Substitute your name, e-mail and the actual name of your package, and
please write a better description.

The maintainer is who releases the package, not necessarily the
original author.  You should generally add one or more ``<author>``
tags, giving appropriate credit::

  <author>Dennis Richey</author>
  <author>Ken Thompson</author>

Also, please provide some URL tags to help users find documentation
and report problems::

  <url type="website">http://ros.org/wiki/camera1394</url>
  <url type="repository">https://github.com/ros-drivers/camera1394.git</url>
  <url type="bugtracker">https://github.com/ros-drivers/camera1394/issues</url>


Metapackages
::::::::::::

These are special-purpose catkin packages for grouping other packages.
Users who install a metapackage binary will also get all packages
directly or indirectly included in that group.  Metapackages must
not install any code or other files, the ``package.xml`` gets
installed automatically.  They can depend on other metapackages, if
desired, but regular catkin packages may not.

Metapackages can be used to resolve stack_ dependencies declared by
legacy rosbuild_ packages not yet converted to catkin.  Catkin
packages should depend directly on the packages they use, not on any
metapackages.

A good use for metapackages is to group the major components of your
robot and then provide a comprehensive grouping for your whole system.

In addition to the XML elements mentioned above, a metapackage
``package.xml`` must contain this::

  <export>
    <metapackage/>
  </export>

In addition to the required ``<buildtool_depend>`` for catkin,
metapackages list the packages in the group using ``<run_depend>``
tags::

  <run_depend>your_custom_msgs</run_depend>
  <run_depend>your_server_node</run_depend>
  <run_depend>your_utils</run_depend>

Metapackages must not include any other ``package.xml`` elements.
But, a ``CMakeLists.txt`` is required, as shown below.

CMakeLists.txt
::::::::::::::

All catkin ``CMakeLists.txt`` files should begin with the following
CMake commands::

  cmake_minimum_required(VERSION 2.8.3)
  project(your_package)

Substitute the actual name of your package in the ``project()``
command.

Metapackage ``CMakeLists.txt`` files should contain only these two
additional lines::

  find_package(catkin REQUIRED)
  catkin_metapackage()

Regular catkin packages generally provide additional information for
dependencies, building targets, installing files and running tests.
They are *required* to use these two commands, usually with additional
arguments::

  find_package(catkin REQUIRED COMPONENTS ...)
  ...
  catkin_metapackage(...)

:ref:`how_to_do_common_tasks_1` pages describe those tasks in detail.
Follow them, observing the usual order of ``CMakeLists.txt`` commands:

#. ``cmake_minimum_required()``
#. ``project()``
#. ``find_package()``
#. ``add_message_files()``, ``add_service_files()``, ``add_action_files()``
#. ``generate_messages()``
#. ``catkin_package()``
#. ``add_library()``, ``add_executable()``, ``target_link_libraries()``
#. ``install()``
#. ``catkin_add_gtest()``, ``catkin_add_nosetests()``, ``add_rostest()``

.. _package.xml: http://www.ros.org/wiki/catkin/package.xml
.. _REP-0127: http://ros.org/reps/rep-0127.html
.. _rosbuild: http://www.ros.org/wiki/rosbuild
.. _stack: http://www.ros.org/wiki/Stacks

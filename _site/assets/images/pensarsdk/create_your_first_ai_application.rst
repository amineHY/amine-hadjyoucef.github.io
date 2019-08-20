========================================
Create your first AI application
========================================

Introduction
------------
In this tutorial, we walk you through the development of an object detection application. The application performs three main tasks:

  * Object detection
  * Filtering of objects
  * Overlay visualization
  * Additional button in the GUI to turn on/off filtering

Workflow of creating an AI application
---------------------------------------
Creating your application requires to:

 * prepare a skeleton project that includes all needed files to support development on your host and docker containers
 * use one of the provided neural networks or add your own
 * create your own application logic, reusing existing python plugins and/or creating your own plugins
 * design your own GUI (if needed) to control your plugins


This tutorial will guide you through each of these steps.



Preparing the skeleton project
------------------------------

In this chapter we will create a new application called "MyApp". To do so, it is recommended that you use the included script `new_app_skeleton.sh` that is present in the folder `~/pensarsdk/applications`. The script will create for you a new folder containing all the boilerplate files and settings for an easier beginning and to make sure the application runs in PensarSDK.

You should move to the application folder and execute the file

.. code-block:: bash

  (ARE SURE GIACOMO)
  user@user-host:~/checkout/plugin-manager/applications$ ./new_app_skeleton.sh

(AMINE I suugest)

.. code-block:: bash

  amine@amine-GS65-Stealth-9SD:~/pensarsdk/applications$ ./new_app_skeleton.sh

  This script can be used to create a new plugin-manager application
   skeleton with a custom name, based on the hello_world example.

  The name must be given in snake case (https://en.wikipedia.org/wiki/Snake_case)
   and the script will take care of appropriate CamelCase conversion
   for class names.

  The skeleton app will be composed of a single python plugin, with an
   empty .kv GUI file, no neural network application and will be
   equivalent in functionality to the hello_world application.

  Please enter the name of the app you want to create:
  my_app

You will find your new application in a folder ``/home/user/pensarsdk/applications/my_app``.

**Note:** Notice that an application name in snake case convention is required by the script. This is used for files and folder names, while `CamelCase` is used for class names and application title. Case conversion is performed automatically by the script when needed.


The skeleton of the created project is shown in the image

.. image:: _static/images/my_app_11.png
  :width: 20 cm
  :align: center


Use one of the provided neural networks or add your own
-------------------------------------------------------

You can use see the full list of all installed models from the section `Framework Neural Networks` on the Explorer tab of VSCode.

.. image:: _static/images/my_app_12.png
  :width: 20 cm
  :align: center






Create your own application logic, reusing existing python plugins and/or creating your own plugins
-------------------------------------------------------------------------------------------------------


Insert schema for the application

Configure the application
^^^^^^^^^^^^^^^^^^^^^^^^^

We now have to configure the application. To do that simply edit the file `my_app.xml`.

.. code-block:: xml

 <?xml version="1.0" encoding="UTF-8"?>
  <plugin_manager_pipeline>

      <title>[b][color=ff0000]MyApp[/color][/b]</title>

      <preprocessor_pipeline>
          <video_capture selected="0" />
          <neural_network type="Darknet" confidence="0.6" frame_resize="1.0" time_subsampling="1">
              <load_params>[darknet/cfg/yolov2-tiny-voc.cfg, darknet/yolov2-tiny-voc.weights]</load_params>
              <labels>darknet/data/voc.names</labels>
          </neural_network>
      </preprocessor_pipeline>

      <plugins_pipeline>
          <plugin modulename="my_app" classname="MyApp" enabled="True"/>
      </plugins_pipeline>

  </plugin_manager_pipeline>

As you can see, it is already filled for you. All you have to do is to configure it according to your application, in particular, the `preprocessor_pipeline` and the `plugins_pipeline`

1. Insert a title for your application, simple text-markup can be used

  .. code-block:: xml

    <title>[b][color=ff0000]MyApp[/color][/b]</title>

2. In the `preprocessor_pipeline` insert the path of the object detection model. For instance, a pre-trained model of YOLOv3 is already installed in the SDK.

  * `darknet/cfg/yolov2-tiny-voc.cfg`
  * `darknet/yolov2-tiny-voc.weights`
  * `darknet/data/voc.names`

    **Note:** You can see the full list of all installed models from the section `Framework Neural Networks` on the Explorer tab of VSCode.

3. Regarding the plugins_pipeline, simply enter the name of each plugin `modulename` in the correct order.

  **Note:**

    * There is no need to add the extension `.py` to the plugin `modulename`

    * `classname` is the name of the class inside the .py

    * At any time, you can enable/disable the plugin usage by setting `enabled` variable to `True` of `False`.

  .. code::  xml

   <plugin modulename="filter" classname="Filter" enabled="True" />
    <plugin modulename="visualize" classname="Visualize" enabled="True"


.. image:: _static/images/my_app_13.png
  :width: 20 cm
  :align: center





Finally, we end-up with a configuration file that looks like follows. we added some comments to clarify the file structure

  .. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
    <plugin_manager_pipeline>

        # Insert a title for your application, simple text-markup can be used (i.e. [/b])
        <title>[b][color=ff0000]MyApp[/color][/b]</title>

        # Configure the preprocessor to perform inference using your neural network or ones of neural
        # networks included in the framework.
        # In this example we use YOLOv2-tiny included in the framework, it is based on DarkNet.
        <preprocessor_pipeline>
            <video_capture selected="0" />
            <neural_network type="Darknet" confidence="0.6" frame_resize="1.0" time_subsampling="1">

                # Specify configuration file (.cgf) and the weights file (.weights), we are using included library.
                <load_params>[darknet/cfg/yolov2-tiny-voc.cfg, darknet/yolov2-tiny-voc.weights]</load_params>

                # Specify label file, we are using included library
    	    <labels>darknet/data/voc.names</labels>

            </neural_network>
        </preprocessor_pipeline>

        # Insert plugins in the right order to build the processing pipe
        <plugins_pipeline>
            # Import library plugin (located in /usr/bin/pensarsdk/plugins/preprocessor_inference.py)
            <plugin modulename="preprocessor_inference" classname="PreProcessorInference" enabled="True" />

            # Add custom (user made) plugins. To be noticed that plugins can include a .kv if a GUI is needed.
            <plugin modulename="filter" classname="Filter" enabled="True" />
            <plugin modulename="visualize" classname="Visualize" enabled="True" />
        </plugins_pipeline>

    </plugin_manager_pipeline>



Create your personalized plugins
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Your can either use the predefined plugins or define yours.


For our application we want to use one installed plugin to perform inference, and create two personalized plugins, `filter.py` and `visualize.py`.

You can find a list of all installed plugins on the left bar of VS Code under   `Framework Plugins` as shown in the image bellow

.. image:: _static/images/my_app_13.png
  :width: 20 cm
  :align: center


You can also define your personalized plugins and use them for several applications.

- `filter.py` - This file is composed of a class, `Filter`, that contains a `process` method it consists of passing through all detection and filter those with a desired label from the label file `voc.names`, for instance, `object_location['label'] == 'person'`.

  .. code-block:: python

    # Name of the plugin: Filter
    class Filter(BoxLayout):
        name = "Filter"

        def process(self, primary_input_frame, secondary_input_frame, output_frame, frame_storage, storage):
            """
            This method filter each frame image and save the results to `frame_storage`.
            Set `desired_label` from the label file, for instance, `person`.
            Other choices: aeroplane, bicycle,bird,boat,bottle,bus,car,cat,chair,cow,
            iningtable, dog,horse,motorbike,person,pottedplant,sheep,sofa,train,tvmonitor.

            Arguments:
                self -- object,
                primary_input_frame -- first input frame,
                secondary_input_frame --  second input frame,
                output_frame -- output frame
                frame_storage -- dict, contains information about the overlay of the output frame
                storage -- dict, ...

            Returns:
                None
            """
            filtered_objects = []

            if 'object_locations' in frame_storage:
                for object_location in frame_storage['object_locations']:
                    if object_location['label'] == 'person':
                        filtered_objects.append(object_location)

            # Save the result in frame_storage
            frame_storage['filtered_objects'] = filtered_objects



 - `visualize.py` - We use OpenCV to draw a rectangle on the filtered objects and some information such as the label and confidence percentage. Here is one way of doing that.

  .. code-block:: python

   class Visualize(BoxLayout):
    name = "Visualize"

    # GUI parameters
    label_text = StringProperty("Display overlay:")
    switch_active = BooleanProperty()

    def process(self, primary_input_frame, secondary_input_frame, output_frame, frame_storage, storage):
        """
        This method performs visualization of overlays for each detected object in each frame image.
        It uses OpenCV function to draw a rectangle and adds a text to display the confidence of the detection.

        """

        if not 'filtered_objects' in frame_storage:
            print('No object detected')
            return

        for filtered_object in frame_storage['filtered_objects']:
            if self.switch_active:
                roi = filtered_object['roi']
                pt1 = (roi[0],roi[1])
                pt2 = (roi[2],roi[3])
                ptText = (roi[0], roi[1] - 20)
                colorOverlay = (0,0,255)
                fontOverlay = cv2.FONT_HERSHEY_SIMPLEX
                textOverlay = "{0:s} {1:2.2f}%".format(filtered_object['label'], filtered_object['confidence']*100)

                cv2.rectangle(output_frame, pt1, pt2, colorOverlay, -4, 8)
                cv2.putText(output_frame, textOverlay, ptText, fontOverlay, 1.0, colorOverlay, 3, 8)







Design your own GUI (if needed) to control your plugins
-------------------------------------------------------

Say we want to personalize the GUI of the application, for instance, you want to create a button to turn on/off filtering

To do that you need to edit the file `visualize.kv` associated to the plugin `visualize.py`


- `visualize.kv`

  .. code-block::

   <Visualize@BoxLayout>:
        orientation: "horizontal"
        size_hint: 1, 1

        switch_active: enable_switch.active

        Label:
            size_hint: 0.5, 1
            text: root.label_text

        Switch:
            id: enable_switch
            size_hint: 0.5, 1
            active: True
Note: Notice that we added GUI parameter to the class of the associated plugin, `visualize.py`

  .. code-block:: python

    # GUI parameters
     label_text = StringProperty("Display overlay:")
     switch_active = BooleanProperty()


Run the application
--------------------

To run the application, two choices are possible, either:

  - use the debug mode by pressing `F5` then again `F5` to launch the application.

  - or you can run the application directly from the terminal by typing

    .. code-block::bash

      run_pensar.sh -a my_app.xml


A GUI should pop-up in full screen running the application we created


.. image::  _static/images/my_app_demo.png
  :width: 20 cm
  :align: center

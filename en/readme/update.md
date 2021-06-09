# Update<a name="EN-US_TOPIC_0000001149909821"></a>

-   [Introduction](#section184mcpsimp)
-   [Directory Structure](#section212mcpsimp)
-   [Repositories Involved](#section251mcpsimp)

## Introduction<a name="section184mcpsimp"></a>

The Update subsystem helps you implement over the air \(OTA\) update of OpenHarmony devices. The update subsystem consists of the following:

1.  Packaging tool

    The packaging tool is developed using Python and deployed on the PC to prepare update packages. It packages each update image, signs the update package, generates the update package execution script, and finally creates an update package. After the execution script is run on a OpenHarmony device, the device parses and executes the script to complete the update process.

    The update package contains two files:  **build\_tools.zip**  and  **update.bin**.

    -   **build\_tools.zip**: update assistance tools, including the executable files and scripts.
    -   **update.bin**: TLV-encoded file, in which update contents are serialized and stored in the TLV format.

    The packaging tool signs the  **update.bin**  file and the generated update package \(**.zip**  file\) independently.

2.  Update service

    The update service is used to search, download, and trigger updates.

3.  Updater

    The updater is the core module of the update subsystem. It provides the following functions:

    1.  Obtains update commands from the misc partition and executes different tasks depending on the commands.
    2.  Decompresses the update package and verifies its validity.
    3.  Starts the update process and parses the update script.
    4.  Installs the related component packages based on the update script.
    5.  Performs post-processing after the update is complete, for example, deleting the update package and recording the update status.

4.  Update app

    The upgrade app is used to trigger search and download of update packages.


Before you get started, familiarize yourself with the following concepts:

-   OTA

    OTA is an implementation of update over a wireless connection. The update package is downloaded to the device through a wireless connection. The device then performs update through the update subsystem.

-   Full package

    A full package is actually a complete image. The update subsystem writes the full package to a partition to update this partition.

-   Differential package

    A differential package is created based on the difference data of two specific versions \(source version and target version\), which is generated by using bsdiff.


## Directory Structure<a name="section212mcpsimp"></a>

```
base/update             # Update subsystem repository
├── app         		# Update app code
├── packaging_tools     # Packaging tool code
├── updater    			# Updater code
│   ├── interfaces  	# External APIs
│   ├── resources 		# UI image resources of the update subsystem, such as animations and progress bar images
│   ├── services  		# Updater logic code
│   └── utils  			# Common code of the update subsystem, including the string processing functions and file processing functions
└── updateservice 		# Update service code
```

## Repositories Involved<a name="section251mcpsimp"></a>

**Update subsystem**

update\_app

update\_updateservice

update\_updater

update\_packaging\_tools

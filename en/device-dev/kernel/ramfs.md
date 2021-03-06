# RAMFS<a name="EN-US_TOPIC_0000001052810480"></a>

-   [Overview](#section7216205735619)
-   [Important Notes](#section970375615711)
-   [Development Guidelines](#section18433111125812)

## Overview<a name="section7216205735619"></a>

RAMFS is a RAM-based file system whose size can be dynamically adjusted. RAMFS does not have a backup storage source. Directory entries and page caches are allocated when files are written into RAMFS. However, data is not written back to any other storage medium, and data is lost after a power outage.

RAMFS stores all files in RAM, and read/write operations are performed in RAM. RAMFS is generally used to store temporary data or data that needs to be frequently modified, such as the  **/tmp**  and  **/var**  directories. Using RAMFS reduces the read/write loss of the memory and improves the data read/write speed.

RAMFS in the OpenHarmony kernel is a simple file system and provides a storage buffer for RAM-based dynamic file systems.

RAMFS in the OpenHarmony kernel is based on VFS and cannot be formatted.

## Important Notes<a name="section970375615711"></a>

-   The read and write pointers of RAMFS are not separated. Therefore, after a file is opened in  **O\_APPEND**  mode, the read pointer is also at the end of the file. You must manually set the position of the read pointer before reading the file.

-   A RAMFS file system can be mounted only once. After it is mounted to a directory, it cannot be mounted to other directories.

-   The number of RAMFS files is restricted by semaphore resources and cannot exceed the value of  **LOSCFG\_BASE\_IPC\_SEM\_LIMIT**.

-   When you use  **open**  with the parameter  **O\_TRUNC**  to open a file, the file content will be cleared.


-   You can perform the following operation on RAMFS files:  **open**,  **close**,  **read**,  **write**,  **seek**,  **opendir**,  **closedir**,  **readdir**,  **readdir\_r**,  **rewinddir**,  **sync**,  **statfs**,  **remove**,  **unlink**,  **mkdir**,  **rmdir**,  **rename**,  **stat**,  **stat64**,  **seek64**,  **mmap**,  **mount**, and  **umount**.


-   RAMFS is under debugging and disabled by default. Do not use it in formal products.


## Development Guidelines<a name="section18433111125812"></a>

You can mount or unmount RAMFS using either of the following methods:

-   Using code:
    1.  Call the  **mount**  function to mount the RAMFS file system.

        ```
        void ram_fs_init(void) {
             int swRet;
             swRet = mount(NULL, RAMFS_DIR, "ramfs", 0, NULL);
             if (swRet != 0) {
                 dprintf("mount ramfs err %d\n", swRet);
                 return;
             }
             dprintf("Mount ramfs finished.\n");
         }
        ```

        If the following information is displayed during the startup of the OpenHarmony kernel, the RAMFS file system is successfully mounted:

        ```
        Mount ramfs finished
        ```

    2.  Call the  **umount**  function to unmount the RAMFS file system. The following information will be displayed in the OpenHarmony kernel if the RAMFS file system is successfully unmounted.

        ```
        void ram_fs_uninit(void) {
             int swRet;
             swRet = umount(RAMFS_DIR);
             if (swRet != 0) {
                 dprintf("Umount ramfs err %d\n", swRet);
                 return;
             }
             dprintf("Umount ramfs finished.\n");
         }
        ```


-   Using commands:
    1.  Run the  **mount**  command to mount the RAMFS file system. If the following information is displayed, the RAMFS file system is successfully mounted:

        ```
        OHOS # mount 0 /ramfs ramfs
        mount ok
        ```

    2.  Run the  **umount**  command to unmount the RAMFS file system. If the following information is displayed, the RAMFS file system is successfully unmounted:

        ```
        OHOS # umount /ramfs 
        umount ok
        ```




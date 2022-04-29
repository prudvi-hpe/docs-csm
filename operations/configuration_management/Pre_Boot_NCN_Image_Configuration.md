# Pre-Boot Configuration of NCN Images

**NOTE:** Some of the documentation linked from this page mentions use of the BOS service. The use of BOS
is only relevant for booting compute nodes and can be ignored when working with NCN images.

This document descibes the configuration of a Kubernetes NCN image. The same steps are relevant for modifying
a Ceph image.

1. Locate the NCN Image to be Modified

    This example assumes you want to modify the image that is currently in use by NCNs. However, the steps
    are the same for any NCN SquashFS image.

    ```console
    ncn# cray artifacts get ncn-images k8s/<version>/filesystem.squashfs ./<version>-filesystem.squashfs
    ncn# cray artifacts get ncn-images k8s/<version>/kernel ./<version>-kernel
    ncn# cray artifacts get ncn-images k8s/<version>/initrd ./<version>-initrd
    ncn# export IMS_ROOTFS_FILENAME=<version>-filesystem.squashfs
    ncn# export IMS_KERNEL_FILENAME=<version>-kernel
    ncn# export IMS_INITRD_FILENAME=<version>-initrd
    ```

1. [Import External Image to IMS](../image_management/Import_External_Image_to_IMS.md)

    This document will instruct the admin to set several environment variables, including the three set in
    the previous step.

1. [Create and Populate a VCS Configuration Repository](Create_and_Populate_a_VCS_Configuration_Repository.md)

1. [Create a CFS Configuration](Create_a_CFS_Configuration.md)

1. [Create an Image Customization CFS Session](Create_an_Image_Customization_CFS_Session.md)

1. Download the Resultant NCN Artifacts

    ```console
    ncn# cray artifacts get boot-images $IMS_RESULTANT_IMAGE_ID/rootfs kubernetes-<version>-1.squashfs
    ncn# cray artifacts get boot-images $IMS_RESULTANT_IMAGE_ID/initrd initrd.img-<version>-1.xz
    ncn# cray artifacts get boot-images $IMS_RESULTANT_IMAGE_ID/kernel 5.3.18-150300.59.43-default-<version>-1.kernel
    ```

1. Upload NCN boot artifacts into S3

    ```console
    ncn# cray artifacts create ncn-images k8s/<version>/filesystem.squashfs kubernetes-<version>-1.squashfs
    ncn# cray artifacts create ncn-images k8s/<version>/initrd.img.xz initrd.img-<version>-1.xz
    ncn# cray artifacts create ncn-images k8s/<version>/kernel <version>-1.kernel
    ```

1. Update NCN Boot Parameters

    - metal.server=http://rgw-vip.nmn/ncn-images/{ceph,k8s}/<version>
    - rd.live.squashimg=<squash_name>
    - kernel=s3://ncn-images/{ceph,k8s}/<version>/kernel
    - initrd=s3://ncn-images/{ceph,k8s}/<version>/initrd

    ```console
    ncn# 
    ```

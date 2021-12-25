Download and Install
====================

Download
########

* Download :download:`version 8.2 <https://github.com/cp2k/cp2k/releases/download/v8.2.0/cp2k-8.2.tar.bz2>` of CP2K.

.. code-block:: Bash

    wget https://github.com/cp2k/cp2k/releases/download/v8.2.0/cp2k-8.2.tar.bz2

.. tip:: 由于CP2K需要较多的依赖包，如果集群可以联网，则在编译过程中可以直接自动下载，否则需要提前下载好对应版本的依赖项安装包，安装包的版本可以在cp2k-8.2/tools/toolchain/scripts/文件夹下对应的install_*.sh文件中找到，下载好后放入 **cp2k-8.2/tools/toolchain/build** 文件夹中后打包上传即可

* 依赖项安装包下载地址 :download:`依赖包 <https://www.cp2k.org/static/downloads/>`

前言
####

CP2K是公认的编译难度较大的计算软件之一，清华大学刘锦程博士已经写了非常详细的 `编译教程 <http://bbs.keinsci.com/thread-19009-1-1.html>`_ 和 `视频 <https://www.bilibili.com/video/BV1Y54y1e7Yx?from=search&seid=18152203032105405918&spm_id_from=333.337.0.0>`_，这里主要记录我根据刘博士的安装教程自己在服务器上安装的过程以及遇到的问题。

编译
####

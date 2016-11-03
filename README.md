# libvot - A C++11 multi-thread library for image retrieval

[![Join the chat at https://gitter.im/hlzz/libvot](https://badges.gitter.im/hlzz/libvot.svg)](https://gitter.im/hlzz/libvot?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://travis-ci.org/hlzz/libvot.svg?branch=master)](https://travis-ci.org/hlzz/libvot) 
[![Build Status](https://travis-ci.org/hlzz/libvot.svg?branch=feature)](https://travis-ci.org/hlzz/libvot) 
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)
[![CoverityScan](https://scan.coverity.com/projects/8983/badge.svg)](https://scan.coverity.com/projects/hlzz-libvot)
### [中文简介](doc/README_Chinese.md)

## Introduction
*libvot* is a fast implementation of vocabulary tree, which is an algorithm widely used in image retrieval and computer vision. It usually comprises three components to build a image retrieval system using vocabulary tree: build a k-means tree using sift descriptors from images, register images into the database, query images against the database. In this library, we use C++11 standard multi-thread library to accelerate the computation, which achieves fast and accurate image retrieval performance. Currently this library is under active development for both research and production. If you find this repository useful, please star it to let me know. :)

## Installation
The build system of libvot is based on [CMake](http://cmake.org). To take full advantages of the new features in C++11, we require the version of CMake to be 2.8.12 or above. Current we have tested our project under Linux (Ubuntu 14.04, CentOS 7) and MacOS (10.10) using gcc. The common steps to build the library are:

1. Extract source files.
2. Create build directory and change to it.
3. Run CMake to configure the build tree.
4. Build the software using selected build tool.
5. Run "make test"
6. See src/example for the usage of this library.

On Unix-like systems with GNU Make as the build tool, the following sequence of commands can be used to compile the source code.

    $ cd libvot
    $ git submodule init & git submodule update  
    $ mkdir build && cd build
    $ cmake ..
    $ make && make test


### Optional Dependencies
* Boost (>1.55), for serialization, python-binding, etc.
* OpenCV (>2.4), for feature detector and general utilities for image processing.
* NVIDIA's Cuda Toolkit 7.5, for GPU-related code.
* NVIDIA's cuDNNv5 for CUDA 7.5, for the deep learning module.

See the [installation](doc/installation.md) guide for details.

### Docker Installation
Besides, *libvot* supports docker installation. [Docker](https://www.docker.com/) is a system to build self-contained versions of a Linux operating system running on your machine. You can pull the latest auto-build [here](https://hub.docker.com/r/hlzz/libvot/). If you encounter any problem building this software on a clean linux OS, [Dockerfile](Dockerfile) is a minimum ubuntu configuration and a good reference.

## First try
Suppose `$LIBVOT_ROOT` represents the root directory of libvot, and it is successfully compiled in `build` subdirectory. You can use `./libvot_feature <image_list>` to first generate a set of descriptor files and use them as inputs to `image_search`. For example, you have some target .jpg image files to generate sift files. Just `cd` into that directory, prepare the `image_list`, and generate sift files in the same directory as the image files:

    $ ls -d $PWD/*.jpg > image_list
    $ $LIBVOT_ROOT/build/bin/libvot_feature <image_list>

Then you can run *image_search* in src/example to generate the image retrieval results
The usage is simply “./image_search <sift_list> <output_dir> [depth] [branch_num] [sift_type] [num_matches] [thread_num]”. 
We add a small image dataset [fountain-P11](http://cvlabwww.epfl.ch/data/multiview/denseMVS.html) to illustrate this process. 
*test_data/fountain_dense* folder contains the sift files generated by `libvot_feature`, while the original images are not included in order to save space. 
If you use the out-of-source build as shown in the installation section and in the *build* directory, 
the following command should work smoothly and generate several output files in `build/bin/vocab_out` directory (you need to change the prefix of filepaths in `test_data/fountain_dense/sift_list`).

    $ cd bin
    $ ls -d $PWD/*.sift > sift_list
    $ ./image_search <sift_list> <output_folder> [depth] [branch_num] [sift_type] [num_matches] [thread_num]  
    $ (e.g.) ./image_search ../../test_data/fountain_dense/sift_list ./vocab_out

Each line in *match.out* contains three numbers “first_index second_index similarity score”. 
Since the library is multi-threaded, the rank is unordered with respect to the first index (they are ordered w.r.t the second index). 
*match_pairs* saves the ordered similarity ranks, from *0*th image to *n-1*th image. Besides, libvot also supports sift files generated by [openMVG](https://github.com/openMVG/openMVG/) (set [sift_type] to 1).

## Documentation
The [homepage](http://hlzz.github.io/libvot/) of libvot is hosted by github-pages. See the documentation [here](http://hlzz.github.io/libvot/doc/html/index.html).

## Contributing
We are working toward the next major release (0.2.0). 
If you are interested in contributing, please have a look at [Roadmap.md](doc/Roadmap.md) and our [Coding style](doc/coding_style.md). 
All types of contributions, including documentation, testing, and new features are welcomed and appreciated.

## References
If you find this library useful for your research, please cite 
```
@inproceedings{shen2016graph,  
  title={Graph-Based Consistent Matching for Structure-from-Motion},  
  author={Shen, Tianwei and Zhu, Siyu and Fang, Tian and Zhang, Runze and Quan, Long},  
  booktitle={European Conference on Computer Vision},  
  pages={139--155},  
  year={2016},  
  organization={Springer}  
}
```
*Note: The image retrieval part of the above research depends on libvot. The functioning graph matching algorithm is in preparation and is planned to be merged into the master branch. For an early preview and implementation details, please send your request to <tshenaa@ust.hk>.*

## License
The BSD 3-Clause License

## Contact and Donation
For inquiries and suggestions, please send your emails to <tshenaa@ust.hk>.

If you would like to support this project, you can contribute to this project, or make a donation via [pledgie](https://pledgie.com/campaigns/30901). Thanks!

<a href='https://pledgie.com/campaigns/30901'><img alt='Click here to lend your support to: Open-Source Image Retrieval Project and make a donation at pledgie.com !' src='https://pledgie.com/campaigns/30901.png?skin_name=chrome' border='0' ></a>

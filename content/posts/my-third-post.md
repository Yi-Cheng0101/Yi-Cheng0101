---
title: "STUDENT CLUSTER COMPETITION"
featured_image: '/images/IMG_0297.jpg'
description: "Welcome to my blog with some of my work in HPC Competition. "
---



# National Tsing Hua University Student Cluster Competition Team
##  2020 APAC HPC-AI Competition               
#####  -  Contestant          Singapore        Oct 2020
   * ####  High Performance and Artificial Intelligence supercomputers Competition.
   * ####  Co-organized by the HPC-AI Advisory Council and the Singapore National Supercomputing Center.
   * ####  Accelerate the ocean science model NEMO ( Nucleus for European Modelling of the Ocean ) from single node ( 24 cpus per node ) to 32 nodes ( 15 CPUs per node ).   
## ASC 20-21 Student Supercomputer Challenge   
####   -  Student Coach       China            Jan 2021
   * ####  The world’s largest supercomputing hackathon.
   * ####  Co-organized by the Asia Supercomputer Community, Inspur Group and Southern University of Science and Technology.
   * ####  Findnding valid dataset, training the NLP model on multiple GPUs and tuning the performance to make the accuracy higher on Cloze test.
## ISC 2021 Student Cluster Competition        
####   -  Contestant          Germany          Jun 2021         
   * ####  Co-organized by the HPC-AI Advisory Council and ISC Group.
   * ####  Accelerate the GPAW which is an open source program package for quantum mechanical atomistic simulations.
   * ####  Using MPI and OpenMP and depending on the input data set to scaled the model on thousands of CPUs.

















# HPC AI NEMO Application

## About HPC-AI competition
Co-organized by the HPC-AI Advisory Council and the Singapore National Supercomputing Center, the 2020 APAC HPC-AI Competition ran from May 20, 2020 through October 15, 2020
## Part 2 – High-Performance Computing – NEMO Climate Simulation Application
* ### Introduction NEMO ( Nucleus for European Modelling of the Ocean )
    NEMO which stands for Nucleus for European Modelling of the Ocean, is a state of-the-art modelling framework for research activities and forecasting services in ocean and climate sciences, developed in a sustainable way by a European consortium. (https://www.nemo-ocean.eu/) 
NEMO is distributed with several reference configurations, allowing both the user to set up a first application and the developer to validate the developments.
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_1.png" width="50%" height="50%" />
    </div>
* ### Benckmark GYR
    The GYRE configuration has been built to simulate the seasonal cycle of a double-gyre box model. 
The domain geometry is a closed rectangular basin on the beta-plane centred at sin(30) and rotated by 45, 3180 km long, 2120 km wide and 4 km deep. The domain is bounded by vertical walls and by a flat bottom. The configuration is meant to represent an idealized North Atlantic or North Pacific basin. The circulation is forced by analytical profiles of wind and buoyancy fluxes.
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_0.png" width="60%" height="60%" />
    </div>
* ### Hotspot Analysis
    * Profiler: vtune
    * __traadv_fct_MOD_tra_adv_fct is the main hotspot
        <div align="center">
        <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_2.png" width="60%" height="60%" />
        </div>
* ### Multiple nodes
    * We use our script to get each hostname of node, and try to cross nodes from one node started. We get errors when running NEMO using 17 nodes and each node has 24 CPUs. The execute program always failed and couldn't be run successfully. The question confused us for a long time, we were very confused about why the program couldn’t be run successfully when using more than 384 CPUs. After our team discussion, our conclusion is that the spec of the test case “nn_Gyre” has limited the number of CPUs. For the program parameter “nn_Gyre” 25 only allows 384 CPUs to run. If we want to run more than 16 nodes with 384 CPUs in total, we should distribute 384 processes equally to each node. Therefore, 384 CPUs distribute to 16 nodes and each node has 12 CPUs. After our experiment (Figure 2.5), using 32 nodes 12 CPUs per node could be run successfully and was the fastest one compared with the other versions using 384 CPUs.
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_6.png" width="60%" height="60%" />
    </div>
    However, we received the mail from NSCC organizer, they told us that NEMO can’t run with an arbitrary number of MPI ranks. After we know that, we continuously test 32 nodes with more than 12 CPUs per node. The result shows that only four various numbers of CPUs per node could be run.

* ### -O option flag 
    * <div align="center">
      <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_7.png" width="60%" height="60%" />
      </div>
  Compiling the makenemo file using arch-linux_gfortran.fcm file has many option flags and we discovered that %CFLAGS and %FCFLAGS there are two flags that could be fixed and may have a chance to optimize the program. The O2 O3 and Ofast compare with 16 nodes each node has 24 processes.(Figure 2.7) The testing result is that the Ofast flag is faster than the O2 flag and O3 is the slowest of them. When we test 32 nodes (12 PCUs per node) using the Ofast flag, the program would fail and can’t be run. After the test, we decided to use default flag O2 to make sure that the answer and output files will be correct and fine.
* ### HDF5  version
    * <div align="center">
      <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_8.png" width="70%" height="70%" />
      </div>
* ### Process-core Binding
    * <div align="center">
      <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_9.png" width="70%" height="70%" />
      </div>
* ### Software Compilation Version
    NSCC machine:
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_3.png" width="50%" height="50%" />
    </div>
* ### Result
    After the above testing, we finally choose the best performance version to hand out.  We select the top two versions among all versions tested, and we test many times and compare their average running time to choose one of them as our best version. The best one is using 32 nodes(15 per CPUs).
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_4.png" width="70%" height="70%" />
    </div>
    <div align="center">
    <img src="https://github.com/Yi-Cheng0101/HPC-AI-NEMO-Application/blob/master/nemo_img_5.png" width="70%" height="70%" />
    </div>


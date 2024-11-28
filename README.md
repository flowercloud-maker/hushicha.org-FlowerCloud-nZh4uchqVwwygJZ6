
# 技术背景


CudaSPONGE是基于CUDA C开发的一款纯GPU分子动力学模拟软件，具有模块化和高性能的特点。官方基本介绍内容如下：



> 分子动力学（Molecular Dynamics, MD）模拟是化学、物理学、生物学、材料科学和许多其他领域的有用工具。在过去 40 年中，人们开发了各种高效的计算算法和MD程序，用于研究日益复杂和大型系统的动力学，如RNA 聚合酶、细胞膜中的膜蛋白、SARS\-CoV\-2病毒等。然而，随着应用范围和规模的扩大，分子模拟软件需要更高的计算能力。缩小模拟与实验之间差距的最直接策略是利用更强大的计算硬件。例如，Shaw研究所专门设计了安东（Anton），可以对系统大小为几百万个原子的单结构域蛋白质进行毫秒级模拟。相比之下，使用图形处理单元（GPU）可能是大多数研究小组最经济实惠和最有前途的方法。从另一个方面看，许多先进的计算算法也已开发出来并得到广泛应用，从而延长了模拟时间尺度。特别是在过去几十年中，人们开发了许多增强型采样方法，以实现快速热力学和/或动力学计算。这些方法包括但不限于广泛使用的伞状采样、元动力学、加速MD、复制交换分子动力学（REMD）、并行回火、模拟回火、多正则模拟（特别是通过Wang\-Landau算法实现）以及许多其他方法。
> 
> 
> 在过去的 15 年中，我们致力于开发面向复杂化学和生物系统的高效分子模拟方法，设计了一系列增强型采样方法，实现了构象和轨迹空间的快速采样，并实现了复杂系统热力学和动力学特性的快速计算。 最近，我们开发了一个名为 SPONGE的国产MD模拟软件包，它不仅实现了GPU加速的传统MD模拟，还实现了我们课题组提出的高效增强采样方法。 该软件包具有高度模块化的特点，可以轻松集成其他功能或算法，尤其是最新的深度学习潜力和算法。


# 安装介绍


首先进到[CudaSPONGE官网](https://github.com)，找到最新版本的软件包下载到本地目录，主要依赖于`nvcc`进行编译构建。解包之后可以看到这样的目录：



```
$ ll
total 220
drwxrwxrwx 1 root root  4096 Sep 11 08:19 ./
drwxrwxrwx 1 root root  4096 Sep 11 08:19 ../
drwxrwxrwx 1 root root  4096 Sep 11 08:19 Lennard_Jones_force/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 MD_core/
-rwxrwxrwx 1 root root  3072 Jan  1  2024 Makefile*
drwxrwxrwx 1 root root  4096 Sep 11 08:19 No_PBC/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 PME_force/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 SITS/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 angle/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 barostats/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 bias/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 bond/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 cmap/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 collective_variable/
-rwxrwxrwx 1 root root  8311 Jan  1  2024 common.cu*
-rwxrwxrwx 1 root root 58097 Jan  1  2024 common.cuh*
drwxrwxrwx 1 root root  4096 Sep 11 08:19 constrain/
-rwxrwxrwx 1 root root 41504 Jan  1  2024 control.cu*
-rwxrwxrwx 1 root root  9465 Jan  1  2024 control.cuh*
drwxrwxrwx 1 root root  4096 Sep 11 08:19 covid-tip4p/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 dihedral/
-rwxrwxrwx 1 root root 34743 Jan  1  2024 jit.cuh*
-rwxrwxrwx 1 root root 32719 Jan  1  2024 main.cu*
-rwxrwxrwx 1 root root  2011 Jan  1  2024 main.cuh*
-rwxrwxrwx 1 root root  3408 Jan  1  2024 main_ti.cu*
-rwxrwxrwx 1 root root   489 Jan  1  2024 main_ti.cuh*
-rwxrwxrwx 1 root root   492 Jan  1  2024 mdin.txt*
drwxrwxrwx 1 root root  4096 Sep 11 08:19 nb14/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 neighbor_list/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 plugin/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 restrain/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 thermostats/
drwxrwxrwx 1 root root  4096 Sep 11 08:19 virtual_atoms/
-rwxrwxrwx 1 root root 11843 Jan  1  2024 vs_project_generator.bat*
drwxrwxrwx 1 root root  4096 Sep 11 08:19 wall/

```

然后可以在这个路径下直接执行：



```
$ make

```

过程中大致输出为：



编译输出

```
$ make
/bin/sh: bc: command not found
/bin/sh: bc: command not found
analyzing dependency of PME_force/cross_PME.cu to PME_force/cross_PME.d
analyzing dependency of PME_force/PME_force.cu to PME_force/PME_force.d
analyzing dependency of Lennard_Jones_force/LJ_soft_core.cu to Lennard_Jones_force/LJ_soft_core.d
analyzing dependency of neighbor_list/neighbor_list.cu to neighbor_list/neighbor_list.d
analyzing dependency of MD_core/MD_core.cu to MD_core/MD_core.d
analyzing dependency of control.cu to control.d
analyzing dependency of common.cu to common.d
analyzing dependency of main_ti.cu to main_ti.d
analyzing dependency of plugin/plugin.cu to plugin/plugin.d
analyzing dependency of wall/soft_wall.cu to wall/soft_wall.d
analyzing dependency of wall/hard_wall.cu to wall/hard_wall.d
analyzing dependency of bias/Meta1D.cu to bias/Meta1D.d
analyzing dependency of bias/steer.cu to bias/steer.d
analyzing dependency of bias/restrain_cv.cu to bias/restrain_cv.d
analyzing dependency of collective_variable/combine.cu to collective_variable/combine.d
analyzing dependency of collective_variable/tabulated.cu to collective_variable/tabulated.d
analyzing dependency of collective_variable/RMSD.cu to collective_variable/RMSD.d
analyzing dependency of collective_variable/simple_cv.cu to collective_variable/simple_cv.d
analyzing dependency of collective_variable/collective_variable.cu to collective_variable/collective_variable.d
analyzing dependency of SITS/SITS.cu to SITS/SITS.d
analyzing dependency of virtual_atoms/virtual_atoms.cu to virtual_atoms/virtual_atoms.d
analyzing dependency of constrain/SHAKE.cu to constrain/SHAKE.d
analyzing dependency of constrain/SETTLE.cu to constrain/SETTLE.d
analyzing dependency of constrain/simple_constrain.cu to constrain/simple_constrain.d
analyzing dependency of constrain/constrain.cu to constrain/constrain.d
analyzing dependency of restrain/restrain.cu to restrain/restrain.d
analyzing dependency of barostats/andersen_barostat.cu to barostats/andersen_barostat.d
analyzing dependency of barostats/Berendsen_barostat.cu to barostats/Berendsen_barostat.d
analyzing dependency of barostats/MC_barostat.cu to barostats/MC_barostat.d
analyzing dependency of thermostats/nose_hoover_chain.cu to thermostats/nose_hoover_chain.d
analyzing dependency of thermostats/Berendsen_thermostat.cu to thermostats/Berendsen_thermostat.d
analyzing dependency of thermostats/Andersen_thermostat.cu to thermostats/Andersen_thermostat.d
analyzing dependency of thermostats/Middle_Langevin_MD.cu to thermostats/Middle_Langevin_MD.d
analyzing dependency of thermostats/Langevin_MD.cu to thermostats/Langevin_MD.d
analyzing dependency of Lennard_Jones_force/pairwise_force.cu to Lennard_Jones_force/pairwise_force.d
analyzing dependency of Lennard_Jones_force/solvent_LJ.cu to Lennard_Jones_force/solvent_LJ.d
analyzing dependency of Lennard_Jones_force/Lennard_Jones_force.cu to Lennard_Jones_force/Lennard_Jones_force.d
analyzing dependency of No_PBC/Lennard_Jones_force_No_PBC.cu to No_PBC/Lennard_Jones_force_No_PBC.d
analyzing dependency of No_PBC/generalized_Born.cu to No_PBC/generalized_Born.d
analyzing dependency of No_PBC/Coulomb_Force_No_PBC.cu to No_PBC/Coulomb_Force_No_PBC.d
analyzing dependency of nb14/nb14.cu to nb14/nb14.d
analyzing dependency of cmap/cmap.cu to cmap/cmap.d
analyzing dependency of dihedral/improper_dihedral.cu to dihedral/improper_dihedral.d
analyzing dependency of dihedral/dihedral.cu to dihedral/dihedral.d
analyzing dependency of angle/Urey_Bradley_force.cu to angle/Urey_Bradley_force.d
analyzing dependency of angle/angle.cu to angle/angle.d
analyzing dependency of bond/listed_forces.cu to bond/listed_forces.d
analyzing dependency of bond/bond_soft.cu to bond/bond_soft.d
analyzing dependency of bond/bond.cu to bond/bond.d
analyzing dependency of main.cu to main.d
/bin/sh: bc: command not found
/bin/sh: bc: command not found
nvcc -o main.o -c main.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o common.o -c common.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o control.o -c control.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o MD_core/MD_core.o -c MD_core/MD_core.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/config.h:27:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config.h:23,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:24,
                 from MD_core/MD_core.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/cpp_dialect.h:131:13: warning: Thrust requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define THRUST_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      THRUST_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                           
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_arch.cuh:36:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/detail/device_synchronize.cuh:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/util.h:36,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/for_each.h:35,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/adl/for_each.h:42,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/for_each.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/for_each.h:277,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.h:104,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/transform.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/transform.h:721,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.inl:31,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.h:87,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/reduce.inl:28,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/reduce.h:781,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.h:61,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/find.inl:25,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/find.h:381,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.h:152,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:1358,
                 from MD_core/MD_core.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_cpp_dialect.cuh:142:13: warning: CUB requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define CUB_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      CUB_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                        
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/config.h:27:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config.h:23,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:24,
                 from MD_core/MD_core.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/cpp_dialect.h:131:13: warning: Thrust requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define THRUST_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      THRUST_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                           
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_arch.cuh:36:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/detail/device_synchronize.cuh:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/util.h:36,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/for_each.h:35,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/adl/for_each.h:42,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/for_each.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/for_each.h:277,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.h:104,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/transform.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/transform.h:721,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.inl:31,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.h:87,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/reduce.inl:28,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/reduce.h:781,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.h:61,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/find.inl:25,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/find.h:381,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.h:152,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:1358,
                 from MD_core/MD_core.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_cpp_dialect.cuh:142:13: warning: CUB requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define CUB_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      CUB_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                        
nvcc -o bond/bond.o -c bond/bond.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o bond/bond_soft.o -c bond/bond_soft.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o bond/listed_forces.o -c bond/listed_forces.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o angle/angle.o -c angle/angle.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o angle/Urey_Bradley_force.o -c angle/Urey_Bradley_force.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o dihedral/dihedral.o -c dihedral/dihedral.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o dihedral/improper_dihedral.o -c dihedral/improper_dihedral.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o cmap/cmap.o -c cmap/cmap.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o nb14/nb14.o -c nb14/nb14.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o neighbor_list/neighbor_list.o -c neighbor_list/neighbor_list.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/config.h:27:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config.h:23,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:24,
                 from neighbor_list/neighbor_list.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/cpp_dialect.h:131:13: warning: Thrust requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define THRUST_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      THRUST_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                           
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_arch.cuh:36:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/detail/device_synchronize.cuh:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/util.h:36,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/for_each.h:35,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/adl/for_each.h:42,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/for_each.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/for_each.h:277,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.h:104,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/transform.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/transform.h:721,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.inl:31,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.h:87,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/reduce.inl:28,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/reduce.h:781,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.h:61,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/find.inl:25,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/find.h:381,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.h:152,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:1358,
                 from neighbor_list/neighbor_list.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_cpp_dialect.cuh:142:13: warning: CUB requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define CUB_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      CUB_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                        
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/config.h:27:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config.h:23,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:24,
                 from neighbor_list/neighbor_list.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/config/cpp_dialect.h:131:13: warning: Thrust requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define THRUST_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      THRUST_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                           
In file included from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_arch.cuh:36:0,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/cub/detail/device_synchronize.cuh:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/util.h:36,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/cuda/detail/for_each.h:35,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/adl/for_each.h:42,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/for_each.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/for_each.h:277,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/transform.h:104,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/transform.inl:27,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/transform.h:721,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.inl:31,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/reduce_by_key.h:87,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/reduce.inl:28,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/reduce.h:781,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.inl:19,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/find.h:61,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/find.inl:25,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/find.h:381,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/system/detail/generic/sort.h:152,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/detail/sort.inl:26,
                 from /usr/local/cuda/bin/../targets/x86_64-linux/include/thrust/sort.h:1358,
                 from neighbor_list/neighbor_list.cu:2:
/usr/local/cuda/bin/../targets/x86_64-linux/include/cub/util_cpp_dialect.cuh:142:13: warning: CUB requires at least C++14. C++11 is deprecated but still supported. C++11 support will be removed in a future release. Define CUB_IGNORE_DEPRECATED_CPP_DIALECT to suppress this message.
      CUB_COMPILER_DEPRECATION_SOFT(C++14, C++11);
             ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~                                                                                                                                                        
nvcc -o No_PBC/Coulomb_Force_No_PBC.o -c No_PBC/Coulomb_Force_No_PBC.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o No_PBC/generalized_Born.o -c No_PBC/generalized_Born.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o No_PBC/Lennard_Jones_force_No_PBC.o -c No_PBC/Lennard_Jones_force_No_PBC.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o Lennard_Jones_force/Lennard_Jones_force.o -c Lennard_Jones_force/Lennard_Jones_force.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o Lennard_Jones_force/solvent_LJ.o -c Lennard_Jones_force/solvent_LJ.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o Lennard_Jones_force/LJ_soft_core.o -c Lennard_Jones_force/LJ_soft_core.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o Lennard_Jones_force/pairwise_force.o -c Lennard_Jones_force/pairwise_force.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o PME_force/PME_force.o -c PME_force/PME_force.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o thermostats/Langevin_MD.o -c thermostats/Langevin_MD.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o thermostats/Middle_Langevin_MD.o -c thermostats/Middle_Langevin_MD.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o thermostats/Andersen_thermostat.o -c thermostats/Andersen_thermostat.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o thermostats/Berendsen_thermostat.o -c thermostats/Berendsen_thermostat.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o thermostats/nose_hoover_chain.o -c thermostats/nose_hoover_chain.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o barostats/MC_barostat.o -c barostats/MC_barostat.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o barostats/Berendsen_barostat.o -c barostats/Berendsen_barostat.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o barostats/andersen_barostat.o -c barostats/andersen_barostat.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o restrain/restrain.o -c restrain/restrain.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o constrain/constrain.o -c constrain/constrain.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o constrain/simple_constrain.o -c constrain/simple_constrain.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o constrain/SETTLE.o -c constrain/SETTLE.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o constrain/SHAKE.o -c constrain/SHAKE.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o virtual_atoms/virtual_atoms.o -c virtual_atoms/virtual_atoms.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o SITS/SITS.o -c SITS/SITS.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o collective_variable/collective_variable.o -c collective_variable/collective_variable.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o collective_variable/simple_cv.o -c collective_variable/simple_cv.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o collective_variable/RMSD.o -c collective_variable/RMSD.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o collective_variable/tabulated.o -c collective_variable/tabulated.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o collective_variable/combine.o -c collective_variable/combine.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o bias/restrain_cv.o -c bias/restrain_cv.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o bias/steer.o -c bias/steer.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o bias/Meta1D.o -c bias/Meta1D.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o wall/hard_wall.o -c wall/hard_wall.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o wall/soft_wall.o -c wall/soft_wall.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o plugin/plugin.o -c plugin/plugin.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
sed -i 's/\r//' mdin.txt
sed -i 's/\r//' covid-tip4p/*
nvcc -o SPONGE  main.o common.o control.o MD_core/MD_core.o bond/bond.o bond/bond_soft.o bond/listed_forces.o angle/angle.o angle/Urey_Bradley_force.o dihedral/dihedral.o dihedral/improper_dihedral.o cmap/cmap.o nb14/nb14.o neighbor_list/neighbor_list.o No_PBC/Coulomb_Force_No_PBC.o No_PBC/generalized_Born.o No_PBC/Lennard_Jones_force_No_PBC.o Lennard_Jones_force/Lennard_Jones_force.o Lennard_Jones_force/solvent_LJ.o Lennard_Jones_force/LJ_soft_core.o Lennard_Jones_force/pairwise_force.o PME_force/PME_force.o thermostats/Langevin_MD.o thermostats/Middle_Langevin_MD.o thermostats/Andersen_thermostat.o thermostats/Berendsen_thermostat.o thermostats/nose_hoover_chain.o barostats/MC_barostat.o barostats/Berendsen_barostat.o barostats/andersen_barostat.o restrain/restrain.o constrain/constrain.o constrain/simple_constrain.o constrain/SETTLE.o constrain/SHAKE.o virtual_atoms/virtual_atoms.o SITS/SITS.o collective_variable/collective_variable.o collective_variable/simple_cv.o collective_variable/RMSD.o collective_variable/tabulated.o collective_variable/combine.o bias/restrain_cv.o bias/steer.o bias/Meta1D.o wall/hard_wall.o wall/soft_wall.o plugin/plugin.o -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o main_ti.o -c main_ti.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o PME_force/cross_PME.o -c PME_force/cross_PME.cu -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50
nvcc -o SPONGE_TI  main_ti.o common.o control.o MD_core/MD_core.o neighbor_list/neighbor_list.o Lennard_Jones_force/LJ_soft_core.o PME_force/PME_force.o PME_force/cross_PME.o -rdc=true -lcudadevrt -lcuda -lcudart -lcufft -lnvrtc --use_fast_math -O4 -ldl -std=c++11 -arch=sm_50 -DCUDA_ARCH_BIN=50

```


即可完成`CudaSPONGE`的编译构建，在当前路径下生成一个`SPONGE`可执行文件：



```
$ ll | grep SPONGE
-rwxr-xr-x 1 root root 32455648 Nov 27 01:44 SPONGE*
-rwxr-xr-x 1 root root  7482232 Nov 27 01:44 SPONGE_TI*

```

# 预处理


为了使得工作空间更加分明，这里我建议先创建一个独立的工作区：



```
$ mkdir work_dir

```

准备一个待模拟的体系，例如我们这里使用一个简单的pdb纯蛋白体系：



```
REMARK   Generated By Xponge (Molecule)
ATOM      1    N ALA     1      -0.095 -11.436  -0.780
ATOM      2   CA ALA     1      -0.171 -10.015  -0.507
ATOM      3   CB ALA     1       1.201  -9.359  -0.628
ATOM      4    C ALA     1      -1.107  -9.319  -1.485
ATOM      5    O ALA     1      -1.682  -9.960  -2.362
ATOM      6    N ARG     2      -1.303  -8.037  -1.397
ATOM      7   CA ARG     2      -2.194  -7.375  -2.328
ATOM      8   CB ARG     2      -3.606  -7.943  -2.235
ATOM      9   CG ARG     2      -4.510  -7.221  -3.228
ATOM     10   CD ARG     2      -5.923  -7.789  -3.136
ATOM     11   NE ARG     2      -6.831  -7.111  -4.087
ATOM     12   CZ ARG     2      -8.119  -7.421  -4.205
ATOM     13  NH1 ARG     2      -8.686  -8.371  -3.468
ATOM     14  NH2 ARG     2      -8.844  -6.747  -5.093
ATOM     15    C ARG     2      -2.273  -5.882  -2.042
ATOM     16    O ARG     2      -1.630  -5.388  -1.119
ATOM     17    N ALA     3      -3.027  -5.119  -2.777
ATOM     18   CA ALA     3      -3.103  -3.697  -2.505
ATOM     19   CB ALA     3      -1.731  -3.041  -2.625
ATOM     20    C ALA     3      -4.039  -3.001  -3.483
ATOM     21    O ALA     3      -4.614  -3.643  -4.359
ATOM     22    N ALA     4      -4.235  -1.719  -3.394
ATOM     23   CA ALA     4      -5.126  -1.057  -4.325
ATOM     24   CB ALA     4      -6.538  -1.625  -4.233
ATOM     25    C ALA     4      -5.205   0.436  -4.039
ATOM     26    O ALA     4      -4.561   0.930  -3.116
ATOM     27  OXT ALA     4      -5.915   1.166  -4.728
TER

```

因为pdb格式经常会面临文件不完整的问题，所以需要使用Xponge进行预处理，安装方法如下：



```
$ python3 -m pip install xponge --upgrade

```

文件处理的流程，这里用`ipython`的交互结果做一个简单的演示：



```
In [1]: import Xponge

In [2]: import Xponge.forcefield.amber.ff14sb # 导入力场
Reference for ff14SB:
  James A. Maier, Carmenza Martinez, Koushik Kasavajhala, Lauren Wickstrom, Kevin E. Hauser, and Carlos Simmerling
    ff14SB: Improving the accuracy of protein side chain and backbone parameters from ff99SB
    Journal of Chemical Theory and Computation 2015 11 (8), 3696-3713
    DOI: 10.1021/acs.jctc.5b00255


In [3]: C = Xponge.load_pdb("case1.pdb", ignore_hydrogen=False, ignore_unknown_name=True) # 加载pdb文件

In [4]: C.add_missing_atoms() # 补齐氢原子和其他丢失的residue atom

In [5]: Xponge.save_pdb(C, "case1_fix.pdb") # 将新的体系保存到pdb文件中

In [7]: !mkdir protein

In [8]: Xponge.save_sponge_input(C, "protein/case1") # 保存SPONGE模拟所需参数文件
Reference for Xponge:
  Xia, Y., Gao, Y. Q.
    Xponge: A Python package to perform pre- and post-processing of molecular simulations
    Journal of Open Source Software, 2022 7(77) 4467
    DOI: 10.21105/joss.04467

Out[8]: Entity of Molecule: case1

In [10]: !ls -l protein/
total 32
-rwxr-xr-x 1 root root 1361 Nov 27 01:34 case1_LJ.txt
-rwxr-xr-x 1 root root 2717 Nov 27 01:34 case1_angle.txt
-rwxr-xr-x 1 root root  185 Nov 27 01:34 case1_atom_name.txt
-rwxr-xr-x 1 root root  153 Nov 27 01:34 case1_atom_type_name.txt
-rwxr-xr-x 1 root root 1437 Nov 27 01:34 case1_bond.txt
-rwxr-xr-x 1 root root  544 Nov 27 01:34 case1_charge.txt
-rwxr-xr-x 1 root root 1639 Nov 27 01:34 case1_coordinate.txt
-rwxr-xr-x 1 root root 4298 Nov 27 01:34 case1_dihedral.txt
-rwxr-xr-x 1 root root  977 Nov 27 01:34 case1_exclude.txt
-rwxr-xr-x 1 root root  371 Nov 27 01:34 case1_mass.txt
-rwxr-xr-x 1 root root 3197 Nov 27 01:34 case1_nb14.txt
-rwxr-xr-x 1 root root   16 Nov 27 01:34 case1_residue.txt
-rwxr-xr-x 1 root root   20 Nov 27 01:34 case1_resname.txt

```

这里除了补齐丢失的原子之外，还涉及到`CudaSPONGE`输入文件的问题，因此用Xponge来处理会相对便捷一些。到这里，就完成了`CudaSPONGE`分子模拟的预处理。


# 能量极小化


对于不同的模拟方法，我们需要给SPONGE输入不同的配置文件，例如能量最小化的一个配置文件的简单形式为`minimize.txt`：



```
case1 Minimization

mode = Minimization 
minimization_dynamic_dt 1
default_in_file_prefix = protein/case1

pbc=0 
cutoff=999

dt = 1e-02
step_limit = 5000
write_information_interval = 50

rst = restart

coordinate_in_file =  protein/case1_coordinate.txt

```

在这个配置文件下，我们执行能量极小化的`mode`，参数文件保存在`protein/`目录下，并以`case1`开头。而模拟系统的笛卡尔坐标，由文件`protein/case1_coordinate.txt`给出。这个设置方便的地方在于，因为我们设置了`restart`参数，所以模拟结束之后会在当前路径下保存一个`restart_coordinate.txt`重启文件，所以下一次启动模拟的时候，只要把启动文件指向到这个restart坐标文件就可以了。配置好参数之后，运行的指令为：



```
$ ../SPONGE -mdin minimize.txt

```

就可以获得相应的输出。


# 其他类型模拟


最常见的就是控温，那么主要就是设定`mode=NVT`，例如这是一个简单的`nvt.txt`，对应SPONGE的mdin输入文件：



```
case1 MD simulation

mode = NVT
default_in_file_prefix = protein/case1

pbc=0 
cutoff=999

dt = 1e-3
step_limit = 50000
write_information_interval = 5000

thermostat = middle_langevin
middle_langevin_gamma = 10

rst = nvt_restart

coordinate_in_file =  restart_coordinate.txt

```

这里起始坐标直接使用能量极小化之后的结果，并将NVT的最终坐标保存到`nvt_restart_coordinate.txt`文件中。其他如NPT模式的配置，可以参考[这篇博客](https://github.com)中的介绍。


# 增强采样


在CudaSPONGE v1\.4的版本中，增强采样方法的定义形式如下`meta.txt`：



```
case1 MD simulation

mode = NVT
default_in_file_prefix = protein/case1

pbc=0 
cutoff=999

dt = 1e-3
step_limit = 50000
write_information_interval = 5000

target_temperature = 300.0
thermostat = middle_langevin
middle_langevin_gamma = 10

rst = meta_restart

coordinate_in_file =  nvt_restart_coordinate.txt
cv_in_file = cv.txt

dont_check_input = 1

```

其中额外包含了一个CV的定义文件`cv.txt`，内容如下：



```
print
{
    CV = torsion
}

torsion
{
   CV_type = dihedral
   atom = 1 3 12 13
}

meta1d
{
    CV = torsion
    dCV = 0.06
    CV_minimal = -3.1416
    CV_maximum = 3.1416
    CV_period = 6.2832
    welltemp_factor = 50
    height = 1
    sigma = 0.3
}

```

一样的运行方法，可以看到输出结果如下：



```
$ ../SPONGE -mdin meta.txt 
SPONGE Version:
    v1.4 2024-01-01

Sub-package:
    SPONGE, for normal molecular dynamics simulations

Citation:
    Huang, Y. - P., Xia, Y., Yang, L., Wei, J., Yang, Y.I.and Gao, Y.Q. (2022), SPONGE: A GPU - Accelerated Molecular Dynamics Package with Enhanced Sampling and AI - Driven Algorithms.Chin.J.Chem., 40 : 160 - 168. https ://doi.org/10.1002/cjoc.202100456


MD TASK NAME:
    case1 MD simulation

START INITIALIZING CONTROLLER
    Start initializing CUDA
        Compiled by CUDA 11.6
        1 device found:
            Device 0:
                Name: NVIDIA GeForce RTX 3080 Ti Laptop GPU
                Memory: 16.0 GB
                Compute Capability: 86
        Set Device 0
    End initializing CUDA
    structure last modify date is 20231219
END INITIALIZING CONTROLLER

START INITIALIZING CV CONTROLLER:
    1 CV defined
    0 cv virtual atoms
END INITIALIZING CV CONTROLLER

START INITIALIZING MD CORE:
    Mode set to NVT
    dt set to 0.001000 ps
    Start reading mass:
    End reading mass

    Start reading charge:
    End reading charge

    Start reading coordinate_in_file:
        atom_numbers is 57
        system start_time is 50.000000
        box_length is
            x: 999.000000
            y: 999.000000
            z: 999.000000
    End reading coordinate_in_file

    Velocity is set to zero as default
    skin set to 2.00 Angstrom
    cutoff set to 999.00 Angstrom
    Start reading excluded list:
        excluded list total length is 290
    End reading excluded list

    Start reading residue list:
        residue_numbers is 4
    End reading residue list

    structure last modify date is 20211105
END INITIALIZING MD CORE

START INITIALIZING MIDDLE LANGEVIN DYNAMICS:
    atom_numbers is 57
    target temperature is 300.00 K
    friction coefficient is 10.00 ps^-1
    random seed is 1476805525
    structure last modify date is 20210826
END INITIALIZING MIDDLE LANGEVIN DYNAMICS

START INITIALIZING LENNADR JONES INFORMATION:
    atom_numbers is 57
    atom_LJ_type_number is 8
    structure last modify date is 20220601
END INITIALIZING LENNADR JONES INFORMATION

START INITIALIZING COULOMB INFORMATION:
    structure last modify date is 20220603
END INITIALIZING COULOMB INFORMATION

START INITIALIZING NB14 (nb14_in_file):
    non-bond 14 numbers is 135
    structure last modify date is 20211222
END INITIALIZING NB14

LISTED FORCES IS NOT INITIALIZED

START INITIALIZING BOND (bond_in_file):
    bond_numbers is 56
    structure last modify date is 20210830
END INITIALIZING BOND

START INITIALIZING BOND SOFT:
END INITIALIZING SOFT BOND

START INITIALIZING ANGLE (angle_in_file):
    angle_numbers is 99
    structure last modify date is 20210830
END INITIALIZING ANGLE

UREY BRADLEY IS NOT INITIALIZED

START INITIALIZING DIHEDRAL (dihedral_in_file):
    dihedral_numbers is 137
    structure last modify date is 20210830
END INITIALIZING DIHEDRAL

IMPROPER DIHEDRAL IS NOT INITIALIZED

START INITIALIZING CMAP:
CMAP IS NOT INITIALIZED

START INITIALIZING HARD WALL:
END INITIALIZING HARD WALL

SOFT WALLS ARE NOT INITIALIZED

RESTRAIN IS NOT INITIALIZED

START INITIALIZING RESTRAIN CV:
    0 CV(s) found for restrain
RESTRAIN CV IS NOT INITIALIZED

START INITIALIZING STEER CV:
    0 CV(s) found for steer
STEER CV IS NOT INITIALIZED

START INITIALIZING META1D:
    1 CV(s) found for meta1d
        CV 0: torsion
        reading 4 atom(s) for torsion
            atom 0: 1
            atom 1: 3
            atom 2: 12
            atom 3: 13
            type of 'torsion' is 'dihedral'
    reading 1 CV_period(s) for meta1d
        CV_period 0: 6.283200
    reading 1 CV_minimal(s) for meta1d
        CV_minimal 0: -3.141600
    reading 1 CV_maximum(s) for meta1d
        CV_maximum 0: 3.141600
    reading 1 dCV(s) for meta1d
        dCV 0: 0.060000
    reading 1 height(s) for meta1d
        height 0: 1.000000
    reading 1 sigma(s) for meta1d
        sigma 0: 0.300000
    reading 1 welltemp_factor(s) for meta1d
        welltemp_factor 0: 50.000000
    Potential update interval is set to write_information_interval by default
    potential output file: meta1d_potential.txt
END INITIALIZING META1D

VIRTUAL ATOM IS NOT INITIALIZED

START INITIALIZING CV PRINTER:
    1 CV(s) found for print
        CV 0: torsion
            type of 'torsion' is 'dihedral'
END INITIALIZING CV PRINTER

------------------------------------------------------------------------------------------------------------
           step =            5000,            time =          55.000,     temperature =          391.02, 
      potential =         -187.55,              LJ =           -6.25,         Coulomb =         -308.21, 
        nb14_LJ =            7.49,         nb14_EE =           23.77,            bond =           19.29, 
          angle =           31.16,        dihedral =           43.88,          meta1d =            1.32, 
        torsion =         -2.3809, 
------------------------------------------------------------------------------------------------------------
           step =           10000,            time =          60.000,     temperature =          298.28, 
      potential =         -183.35,              LJ =           -5.40,         Coulomb =         -316.07, 
        nb14_LJ =            9.65,         nb14_EE =           29.10,            bond =           19.95, 
          angle =           33.31,        dihedral =           43.66,          meta1d =            2.45, 
        torsion =         -2.5452, 
------------------------------------------------------------------------------------------------------------
           step =           15000,            time =          65.000,     temperature =          433.99, 
      potential =         -188.78,              LJ =           -4.70,         Coulomb =         -316.29, 
        nb14_LJ =            8.71,         nb14_EE =           23.72,            bond =           21.29, 
          angle =           26.82,        dihedral =           47.90,          meta1d =            3.77, 
        torsion =         -2.4958, 
------------------------------------------------------------------------------------------------------------
           step =           20000,            time =          70.000,     temperature =          249.71, 
      potential =         -183.19,              LJ =           -5.34,         Coulomb =         -329.13, 
        nb14_LJ =            9.73,         nb14_EE =           31.84,            bond =           22.68, 
          angle =           34.74,        dihedral =           50.28,          meta1d =            2.01, 
        torsion =         -1.9129, 
------------------------------------------------------------------------------------------------------------
           step =           25000,            time =          75.000,     temperature =          355.19, 
      potential =         -190.72,              LJ =           -7.80,         Coulomb =         -314.26, 
        nb14_LJ =            8.21,         nb14_EE =           23.10,            bond =           19.06, 
          angle =           29.59,        dihedral =           48.39,          meta1d =            3.01, 
        torsion =         -1.8429, 
------------------------------------------------------------------------------------------------------------
           step =           30000,            time =          80.000,     temperature =          407.06, 
      potential =         -181.22,              LJ =            0.79,         Coulomb =         -293.82, 
        nb14_LJ =            6.62,         nb14_EE =            5.15,            bond =           24.57, 
          angle =           32.20,        dihedral =           41.94,          meta1d =            1.32, 
        torsion =          0.4504, 
------------------------------------------------------------------------------------------------------------
           step =           35000,            time =          85.000,     temperature =          203.30, 
      potential =         -179.47,              LJ =            0.97,         Coulomb =         -298.49, 
        nb14_LJ =            8.28,         nb14_EE =            4.97,            bond =           27.75, 
          angle =           39.17,        dihedral =           35.27,          meta1d =            2.61, 
        torsion =          0.4398, 
------------------------------------------------------------------------------------------------------------
           step =           40000,            time =          90.000,     temperature =          310.20, 
      potential =         -190.12,              LJ =           -3.52,         Coulomb =         -296.07, 
        nb14_LJ =            9.64,         nb14_EE =           13.64,            bond =           16.37, 
          angle =           28.68,        dihedral =           39.10,          meta1d =            2.03, 
        torsion =         -0.0447, 
------------------------------------------------------------------------------------------------------------
           step =           45000,            time =          95.000,     temperature =          408.99, 
      potential =         -190.56,              LJ =           -2.71,         Coulomb =         -296.55, 
        nb14_LJ =            8.19,         nb14_EE =           10.34,            bond =           19.28, 
          angle =           30.85,        dihedral =           36.49,          meta1d =            3.57, 
        torsion =          0.0334, 
------------------------------------------------------------------------------------------------------------
           step =           50000,            time =         100.000,     temperature =          462.33, 
      potential =         -189.09,              LJ =           -0.75,         Coulomb =         -293.79, 
        nb14_LJ =            9.71,         nb14_EE =            6.49,            bond =           18.99, 
          angle =           27.54,        dihedral =           38.08,          meta1d =            4.63, 
        torsion =         -0.0077, 
------------------------------------------------------------------------------------------------------------
Core Run Wall Time: 13.704983 seconds
Core Run Speed: 315.220093 ns/day
End Pause

```

# 总结概要


本文简单介绍了一下CudaSPONGE高性能分子动力学模拟软件，其基于原生的CUDA C开发，具有极高的模拟效率。结合前处理工具Xponge用于生成和处理输入文件，可以很好的兼容目前常用的很多力场形式。


# 版权声明


本文首发链接为：[https://github.com/dechinphy/p/cudasponge.html](https://github.com)


作者ID：DechinPhy


更多原著文章：[https://github.com/dechinphy/](https://github.com):[蓝猫机场加速器](https://dahelaoshi.com)


请博主喝咖啡：[https://github.com/dechinphy/gallery/image/379634\.html](https://github.com)


# 参考链接


1. [https://github.com/bgalang/p/18316362](https://github.com)



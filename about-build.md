SVO_EDGE demo运行

安装ros的kinetic版本（安装方式见ros官网）

1.error:

invalid conversion from ‘const char*’ to ‘int’ [-fpermissive]

outputFile << cv::format(data,"CSV")<<std::endl;

solution:

outputFile << format(m, cv::Formatter::FMT_CSV) << endl;

2.error:

YOUR_FOLDER_NAME/SVO_edgelet/svo_edgelet-master/include/precomp.hpp:52:37: fatal error: opencv2/core/internal.hpp: No such file or directory

solution:

//opencv2/core/internal.hpp

3.error:

YOUR_FOLDER_NAME/SVO_edgelet/svo_edgelet-master/src/five-point.cpp:100:25: error: no matching function for call to ‘cv::Mat::Mat(CvMat*&)’

cv::Mat(tempMask).setTo(true);

solution:

cv::cvarrToMat(tempMask)

4.error:

ISO C++ forbids declaration of ‘CV_IMPLEMENT_QSORT’ with no type [-fpermissive]

static CV_IMPLEMENT_QSORT( icvSortDistances, int, CV_LT )

solution:

static void

icvSortDistances( int *array, size_t total, int /*unused*/ )

{

std::sort( &array[0], &array[total] );

}

5.error:

fatal error: vikit/math_utils.h: No such file or directory

solution:

locate vikit,get the path ,then add path to include of cmakelists.txt

/home/ubuntu/catkin_ws/src/rpg_vikit/vikit_common/include/

6.error: no matching function for call to ‘g2o::BlockSolver<g2o::BlockSolverTraits<6, 3> >::BlockSolver(g2o::BlockSolver<g2o::BlockSolverTraits<6, 3> >::LinearSolverType*&)’

solution:

//g2o::BlockSolver_6_3 * solver_ptr = new g2o::BlockSolver_6_3(linearSolver); // line 356

g2o::BlockSolver_6_3 * solver_ptr = new g2o::BlockSolver_6_3(std::unique_ptr<g2o::BlockSolver_6_3::LinearSolverType> (linearSolver));

//g2o::OptimizationAlgorithmLevenberg * solver = new g2o::OptimizationAlgorithmLevenberg(solver_ptr); // line 357

g2o::OptimizationAlgorithmLevenberg * solver = new g2o::OptimizationAlgorithmLevenberg(std::unique_ptr<g2o::BlockSolver_6_3> (solver_ptr));

7.error:

/usr/bin/ld: cannot find -lg2o_core

/usr/bin/ld: cannot find -lg2o_stuff

/usr/bin/ld: cannot find -lg2o_types_slam3d

solution:

#LIST(APPEND LINK_LIBS g2o_core_d g2o_solver_csparse_d g2o_csparse_extension_d g2o_types_sba_d g2o_solver_dense_d g2o_stuff_d g2o_parser_d g2o_solver_pcg_d cholmod cxsparse )

LIST(APPEND LINK_LIBS g2o_core g2o_solver_csparse g2o_csparse_extension g2o_types_sba g2o_solver_dense g2o_stuff g2o_parser g2o_solver_pcg cholmod cxsparse )

8.error:

../lib/libsvo.so: undefined reference to `Sophus::SE3::operator*(Eigen::Matrix<double, 3, 1, 0, 3, 1> const&) const'

solution:

https://www.cnblogs.com/gary-guo/p/9467791.html

安装Sophus时，有个lib文件“libSophus.so”会出现在/usr/local/lib/libSophus.so 时，libSophus.so 应该被链接到 Sophus_LIBRARIES， cmake没链接上。

# Sophus

find_package( Sophus REQUIRED )

set(Sophus_LIBRARIES libSophus.so)

include_directories( ${Sophus_INCLUDE_DIRS} )

SET(Sophus_LIBRARIES libSophus.so)

sudo apt-get install的eigen,ubuntu1604默认是3.2.92，更新到最新，是3.3的beta版本，默认还是3.2.92。由于sophus需要的是3.3eigen，git上下载后手动编译。需要注意的点：配置eigen路径的时候，不用默认的find_package(Eigen Required），直接手动添加eigen的include目录：

include_directory(

/usr/local/include/eigen3

)

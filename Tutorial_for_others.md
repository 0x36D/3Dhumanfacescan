### Colmap 与 MVS 使用教程
确保Colmap与OpenMVS已经安装完成后
##### 1. 准备数据
将待处理的图片放入一个文件夹中，命名为`input`，并将其放置在`Colmap`与`OpenMVS`的同级目录下。其中，`input`文件夹中应包含待处理的图片。
##### 2.提取特征
使用Colmap提取特征。
```
colmap feature_extractor --database_path database.db --image_path input
```
这一步会在目录下生成一个database.db文件。其包含了相应的特征信息。同时feature_extractor还包含一些可以调整的参数。例如，在提取特征时可以限制图片的大小。affine_shape默认为true
```
colmap feature_extractor --database_path database.db --image_path input --SiftExtraction.max_image_size 2000 --SiftExtraction.estimate_affine_shape false --ImageReader.single_camera 1 --ImageReader.camera_model perspective
--SiftExtraction.use_gpu true
```
具体可以查看：
```
colmap feature_extractor --help
```
这里列出默认的参数信息以便参考。
<details>
<summary> 点击展开 </summary>
  <pre><code>
  --random_seed arg (=0)
  --log_to_stderr arg (=1)
  --log_level arg (=0)
  --project_path arg
  --database_path arg
  --image_path arg
  --camera_mode arg (=-1)
  --image_list_path arg
  --descriptor_normalization arg (=l1_root)
                                        {'l1_root', 'l2'}
  --ImageReader.mask_path arg
  --ImageReader.camera_model arg (=SIMPLE_RADIAL)
  --ImageReader.single_camera arg (=0)
  --ImageReader.single_camera_per_folder arg (=0)
  --ImageReader.single_camera_per_image arg (=0)
  --ImageReader.existing_camera_id arg (=-1)
  --ImageReader.camera_params arg
  --ImageReader.default_focal_length_factor arg (=1.2)
  --ImageReader.camera_mask_path arg
  --SiftExtraction.num_threads arg (=-1)
  --SiftExtraction.use_gpu arg (=1)
  --SiftExtraction.gpu_index arg (=-1)
  --SiftExtraction.max_image_size arg (=3200)
  --SiftExtraction.max_num_features arg (=8192)
  --SiftExtraction.first_octave arg (=-1)
  --SiftExtraction.num_octaves arg (=4)
  --SiftExtraction.octave_resolution arg (=3)
  --SiftExtraction.peak_threshold arg (=0.0066666666666666671)
  --SiftExtraction.edge_threshold arg (=10)
  --SiftExtraction.estimate_affine_shape arg (=0)
  --SiftExtraction.max_num_orientations arg (=2)
  --SiftExtraction.upright arg (=0)
  --SiftExtraction.domain_size_pooling arg (=0)
  --SiftExtraction.dsp_min_scale arg (=0.16666666666666666)
  --SiftExtraction.dsp_max_scale arg (=3)
  --SiftExtraction.dsp_num_scales arg (=10)
  </pre></code>

  </details>

##### 3.特征匹配
使用Colmap进行特征匹配。
```
colmap exhaustive_matcher --database_path database.db
```
默认参数包括以下这些，使用方式与上文相同。
<details>
<summary> 点击展开 </summary>
  <pre><code>
  --random_seed arg (=0)
  --log_to_stderr arg (=1)
  --log_level arg (=0)
  --project_path arg
  --database_path arg
  --SiftMatching.num_threads arg (=-1)
  --SiftMatching.use_gpu arg (=1)
  --SiftMatching.gpu_index arg (=-1)
  --SiftMatching.max_ratio arg (=0.80000000000000004)
  --SiftMatching.max_distance arg (=0.69999999999999996)
  --SiftMatching.cross_check arg (=1)
  --SiftMatching.guided_matching arg (=0)
  --SiftMatching.max_num_matches arg (=32768)
  --TwoViewGeometry.min_num_inliers arg (=15)
  --TwoViewGeometry.multiple_models arg (=0)
  --TwoViewGeometry.compute_relative_pose arg (=0)
  --TwoViewGeometry.max_error arg (=4)
  --TwoViewGeometry.confidence arg (=0.999)
  --TwoViewGeometry.max_num_trials arg (=10000)
  --TwoViewGeometry.min_inlier_ratio arg (=0.25)
  --ExhaustiveMatching.block_size arg (=50)
  <pre></code>
  
  </details>

  ##### 4.相机姿态估计
使用Colmap进行相机姿态估计。
```
colmap mapper --database_path database.db --image_path input --output_path output
```
默认参数包括以下这些，使用方式与上文相同。这一步会生成一个output文件夹，其中包含了相机姿态估计的结果。其中包含camera.bin, images.bin, points3D.bin。其中camera.bin 包含了相机的内参和外参。内参是相机的焦距和主点。外参是相机的旋转和平移。也可以保存为.txt 文件。
<details>
<summary> 点击展开 </summary>
  <pre><code>
  --random_seed arg (=0)
  --log_to_stderr arg (=1)
  --log_level arg (=0)
  --project_path arg
  --database_path arg
  --image_path arg
  --input_path arg
  --output_path arg
  --image_list_path arg
  --Mapper.min_num_matches arg (=15)
  --Mapper.ignore_watermarks arg (=0)
  --Mapper.multiple_models arg (=1)
  --Mapper.max_num_models arg (=50)
  --Mapper.max_model_overlap arg (=20)
  --Mapper.min_model_size arg (=10)
  --Mapper.init_image_id1 arg (=-1)
  --Mapper.init_image_id2 arg (=-1)
  --Mapper.init_num_trials arg (=200)
  --Mapper.extract_colors arg (=1)
  --Mapper.num_threads arg (=-1)
  --Mapper.min_focal_length_ratio arg (=0.10000000000000001)
  --Mapper.max_focal_length_ratio arg (=10)
  --Mapper.max_extra_param arg (=1)
  --Mapper.ba_refine_focal_length arg (=1)
  --Mapper.ba_refine_principal_point arg (=0)
  --Mapper.ba_refine_extra_params arg (=1)
  --Mapper.ba_local_num_images arg (=6)
  --Mapper.ba_local_function_tolerance arg (=0)
  --Mapper.ba_local_max_num_iterations arg (=25)
  --Mapper.ba_global_images_ratio arg (=1.1000000000000001)
  --Mapper.ba_global_points_ratio arg (=1.1000000000000001)
  --Mapper.ba_global_images_freq arg (=500)
  --Mapper.ba_global_points_freq arg (=250000)
  --Mapper.ba_global_function_tolerance arg (=0)
  --Mapper.ba_global_max_num_iterations arg (=50)
  --Mapper.ba_global_max_refinements arg (=5)
  --Mapper.ba_global_max_refinement_change arg (=0.00050000000000000001)
  --Mapper.ba_local_max_refinements arg (=2)
  --Mapper.ba_local_max_refinement_change arg (=0.001)
  --Mapper.ba_use_gpu arg (=0)
  --Mapper.ba_gpu_index arg (=-1)
  --Mapper.ba_min_num_residuals_for_cpu_multi_threading arg (=50000)
  --Mapper.snapshot_path arg
  --Mapper.snapshot_images_freq arg (=0)
  --Mapper.fix_existing_images arg (=0)
  --Mapper.init_min_num_inliers arg (=100)
  --Mapper.init_max_error arg (=4)
  --Mapper.init_max_forward_motion arg (=0.94999999999999996)
  --Mapper.init_min_tri_angle arg (=16)
  --Mapper.init_max_reg_trials arg (=2)
  --Mapper.abs_pose_max_error arg (=12)
  --Mapper.abs_pose_min_num_inliers arg (=30)
  --Mapper.abs_pose_min_inlier_ratio arg (=0.25)
  --Mapper.filter_max_reproj_error arg (=4)
  --Mapper.filter_min_tri_angle arg (=1.5)
  --Mapper.max_reg_trials arg (=3)
  --Mapper.local_ba_min_tri_angle arg (=6)
  --Mapper.tri_max_transitivity arg (=1)
  --Mapper.tri_create_max_angle_error arg (=2)
  --Mapper.tri_continue_max_angle_error arg (=2)
  --Mapper.tri_merge_max_reproj_error arg (=4)
  --Mapper.tri_complete_max_reproj_error arg (=4)
  --Mapper.tri_complete_max_transitivity arg (=5)
  --Mapper.tri_re_max_angle_error arg (=5)
  --Mapper.tri_re_min_ratio arg (=0.20000000000000001)
  --Mapper.tri_re_max_trials arg (=1)
  --Mapper.tri_min_angle arg (=1.5)
  --Mapper.tri_ignore_two_view_tracks arg (=1)
  </code></pre>
</details>

##### 5.转换相机参数成为`Pinhole`格式
使用Colmap将相机参数转换为`Pinhole`格式。
```
colmap image_undistorter --image_path input --input_path output --output_path output_undistorted
```
##### （扩展部分）深度学习训练需要的信息。
表面重建常用的框架是Nerf，其需要的基本信息是图片与对应的相机和它的参数，相机模式可以是`Pinhole`或`Radial`等。如果需要的利用深度学习进行重建，到4或者5即可跳转[]
##### 6.利用OpenMVS转换colmap文件生成.mvs文件

```
interfaceCOLMAP -i output_undistorted -o output.mvs
```
其中输入的文件的文件路径是`output_undistorted`，输出的文件是`output.mvs`。

##### 7.利用OpenMVS进行稠密点云生成
使用OpenMVS进行稠密点云生成。这一步将稀疏点云转换为稠密点云。
```
DensifyPointCloud -i output.mvs -o output_dense.mvs
```
<details>
<summary> 点击展开 </summary>
  <pre><code>
    -h [ --help ]                         produce this help message
    -w [ --working-folder ] arg           working directory (default currentdirectory)
    -c [ --config-file ] arg (=DensifyPointCloud.cfg) file name containing program options
    --archive-type arg (=-1)              project archive type: -1-interface, 0-text, 1-binary, 2-compressed binary
    --process-priority arg (=-1)          process priority (below normal by default)
    --max-threads arg (=0)                maximum number of threads (0 for using all available cores)
    -v [ --verbosity ] arg (=2)           verbosity level
    --cuda-device arg (=-1)               CUDA device number to be used for depth-map estimation (-2 - CPU processing, -1 - best GPU, >=0 - device index)

Densify options:
    -i [ --input-file ] arg               input filename containing camera poses and image list
    -p [ --pointcloud-file ] arg          sparse point-cloud with views file name to densify (overwrite existing point-cloud)
    -o [ --output-file ] arg              output filename for storing the dense point-cloud (optional)
    --view-neighbors-file arg           input filename containing the list of views and their neighbors (optional)
    --output-view-neighbors-file arg    output filename containing the generated list of views and their neighbors
    --resolution-level arg (=1)         how many times to scale down the images before point cloud computation
    --max-resolution arg (=2560)        do not scale images higher than this resolution
    --min-resolution arg (=640)         do not scale images lower than this resolution
    --sub-resolution-levels arg (=2)    number of patch-match sub-resolution iterations (0 - disabled)
    --number-views arg (=8)             number of views used for depth-map estimation (0 - all neighbor views available)
    --number-views-fuse arg (=3)        minimum number of images that agrees with an estimate during fusion in order to consider it inlier (<2 - only merge depth-maps)
    --ignore-mask-label arg (=-1)       label value to ignore in the image mask, stored in the MVS scene or next to each image with '.mask.png' extension (<0 - disabled)
    --mask-path arg                     path to folder containing mask images with '.mask.png' extension
    --iters arg (=4)                    number of patch-match iterations
    --geometric-iters arg (=2)          number of geometric consistent patch-match iterations (0 - disabled)
    --estimate-colors arg (=2)          estimate the colors for the dense point-cloud (0 - disabled, 1 - final, 2 - estimate)
    --estimate-normals arg (=2)         estimate the normals for the dense point-cloud (0 - disabled, 1 - final, 2 - estimate)
    --estimate-scale arg (=0)           estimate the point-scale for the dense point-cloud (scale multiplier, 0 - disabled)
    --sub-scene-area arg (=0)           split the scene in sub-scenes such that each sub-scene surface does not exceed the given maximum sampling area (0 - disabled)
    --sample-mesh arg (=0)              uniformly samples points on a mesh (0 - disabled, <0 - number of points, >0 - sample density per square unit)
    --fusion-mode arg (=0)              depth-maps fusion mode (-2 - fuse disparity-maps, -1 - export disparity-maps only, 0 - depth-maps & fusion, 1 - export depth-maps only)
    --postprocess-dmaps arg (=7)        flags used to filter the depth-maps after estimation (0 - disabled, 1 - remove-speckles, 2 - fill-gaps, 4 - adjust-filter)
    --filter-point-cloud arg (=0)       filter dense point-cloud based on visibility (0 - disabled)
    --export-number-views arg (=0)      export points with >= number of views (0 - disabled, <0 - save MVS project too)
    --roi-border arg (=0)               add a border to the region-of-interest when cropping the scene (0 - disabled, >0 - percentage, <0 - absolute)
    --estimate-roi arg (=2)             estimate and set region-of-interest (0 - disabled, 1 - enabled, 2 - adaptive)
    --crop-to-roi arg (=1)              crop scene using the region-of-interest
    --remove-dmaps arg (=0)             remove depth-maps after fusion
    --tower-mode arg (=4)               add a cylinder of points in the center of ROI; scene assume to be Z-uporiented (0 - disabled, 1 - replace, 2 - append, 3 - select neighbors, 4 - select neighbors & append, <0 - force tower mode)
  </code></pre>
</details>


##### 8.利用OpenMVS进行表面重建
使用OpenMVS进行表面重建。这一步将稠密点云转换为表面模型。
```
ReconstructMesh -i output_dense.mvs -o output_dense.ply
```
对应的参数：
<details>
<summary> 点击展开 </summary>
  <pre><code>
  -h [ --help ]                         produce this help message
  -w [ --working-folder ] arg           working directory (default current directory)
  -c [ --config-file ] arg (=ReconstructMesh.cfg)  file name containing program options
  --export-type arg (=ply)              file type used to export the 3D scene (ply or obj)
  --archive-type arg (=4294967295)      project archive type: -1-interface, 0-text, 1-binary, 2-compressed binary
  --process-priority arg (=-1)          process priority (below normal by default)
  --max-threads arg (=0)                maximum number of threads (0 for using all available cores)
  -v [ --verbosity ] arg (=2)           verbosity level
  --cuda-device arg (=-1)               CUDA device number to be used to reconstruct the mesh (-2 - CPU processing, -1 - best GPU, >=0 - device index)

Reconstruct options:
  -i [ --input-file ] arg               input filename containing camera poses and image list
  -p [ --pointcloud-file ] arg          dense point-cloud with views file name to reconstruct (overwrite existing point-cloud)
  -o [ --output-file ] arg              output filename for storing the mesh
  -d [ --min-point-distance ] arg (=2.5) minimum distance in pixels between the projection of two 3D points to consider them different while triangulating (0 - disabled)
  --integrate-only-roi arg (=0)         use only the points inside the ROI
  --constant-weight arg (=1)            considers all view weights 1 instead of the available weight
  -f [ --free-space-support ] arg (=0)  exploits the free-space support in order to reconstruct weakly-represented surfaces
  --thickness-factor arg (=1)           multiplier adjusting the minimum thickness considered during visibility weighting
  --quality-factor arg (=1)             multiplier adjusting the quality weight considered during graph-cut

Clean options:
  --decimate arg (=1)                   decimation factor in range (0..1] to be applied to the reconstructed surface (1 - disabled)
  --target-face-num arg (=0)            target number of faces to be applied to the reconstructed surface. (0 - disabled)
  --remove-spurious arg (=20)           spurious factor for removing faces with too long edges or isolated components (0 - disabled)
  --remove-spikes arg (=1)              flag controlling the removal of spike faces
  --close-holes arg (=30)               try to close small holes in the reconstructed surface (0 - disabled)
  --smooth arg (=2)                     number of iterations to smooth the reconstructed surface (0 - disabled)
  --edge-length arg (=0)                remesh such that the average edge length is this size (0 - disabled)
  --roi-border arg (=0)                 add a border to the region-of-interest when cropping the scene (0 - disabled, >0 - percentage, <0 - absolute)
  --crop-to-roi arg (=1)                crop scene using the region-of-interest
  </code></pre>
</details>

##### 9.平滑mesh表面（这一步耗时最长）
使用OpenMVS进行表面平滑。这一步将表面模型进行平滑。
```
RefineMesh -i output_dense.ply -o output_dense_refined.ply
```
对应的参数：
<details>
<summary> 点击展开 </summary>
  <pre><code>
    ffff
  <pre></code>
</details>



### 注意事项
##### openMVS 的环境变量
在Windows系统中，如果设置了openMVS的环境变量，例如`PATH=E:\facescan\openMVS`则文件的默认路径是`E:\facescan\`在使用`openMVS`时，需要设置文件的绝对路径，默认的路径都是`openMVS`的路径，使用的过程中会报错。重新编译的话需要对这一部分**修改**。

##### 脚本文件
在Windows系统中，可以利用python的subprocess模块来运行脚本文件。具体事例在`~/test.py`中。并且包含了每一步的时间统计。
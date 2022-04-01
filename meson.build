project('bgfx', ['c', 'cpp'], version: '1.115')

add_project_arguments('-DBX_CONFIG_DEBUG', language: 'cpp')

# bx
bx_sources = files('bx/src/amalgamated.cpp')
bx_inc = include_directories('bx/include', 'bx/3rdparty')
bx = static_library('bx', bx_sources, include_directories: bx_inc)
bx_dep = declare_dependency(include_directories: bx_inc, link_with: bx)

# bimg
## 3rdparty
astc = static_library('astc', 'bimg/3rdparty/astc/astc_averages_and_directions.cpp',
                    'bimg/3rdparty/astc/astc_block_sizes2.cpp',
                    'bimg/3rdparty/astc/astc_color_quantize.cpp',
                    'bimg/3rdparty/astc/astc_color_unquantize.cpp',
                    'bimg/3rdparty/astc/astc_compress_symbolic.cpp',
                    'bimg/3rdparty/astc/astc_compute_variance.cpp',
                    'bimg/3rdparty/astc/astc_decompress_symbolic.cpp',
                    'bimg/3rdparty/astc/astc_encoding_choice_error.cpp',
                    'bimg/3rdparty/astc/astc_ideal_endpoints_and_weights.cpp',
                    'bimg/3rdparty/astc/astc_imageblock.cpp',
                    'bimg/3rdparty/astc/astc_integer_sequence.cpp',
                    'bimg/3rdparty/astc/astc_kmeans_partitioning.cpp',
                    'bimg/3rdparty/astc/astc_lib.cpp',
                    'bimg/3rdparty/astc/astc_partition_tables.cpp',
                    'bimg/3rdparty/astc/astc_percentile_tables.cpp',
                     include_directories: 'bimg/3rdparty/astc')

astc_codec = static_library('astc-codec',
                           include_directories: ['bimg/3rdparty', 'bimg/3rdparty/astc-codec', 'bimg/3rdparty/astc-codec/include'])

libsquish = static_library('libsquish', 'bimg/3rdparty/libsquish/alpha.cpp')

tinyexr = static_library('tinyexr', 'bimg/3rdparty/tinyexr/deps/miniz/miniz.c',
                        include_directories: ['bimg/3rdparty/tinyexr', 'bimg/3rdparty/tinyexr/deps/miniz'])

bimg_sources = files('bimg/src/image_cubemap_filter.cpp',
                    'bimg/src/image_decode.cpp', 
                    'bimg/src/image_encode.cpp',
                    'bimg/src/image_gnf.cpp',
                    'bimg/src/image.cpp')
bimg_inc = include_directories('bimg/include', 'bimg/3rdparty')
bimg = static_library('bimg', bimg_sources, dependencies: bx_dep, include_directories: bimg_inc, link_with: [astc, astc_codec, libsquish, tinyexr])

project('bgfx', ['c', 'cpp'], version: '1.115')

add_project_arguments('-DBX_CONFIG_DEBUG', language: 'cpp')

cpp = meson.get_compiler('cpp')

dl = cpp.find_library('dl')
threads = dependency('threads', required: true)
x11 = dependency('X11')
glx = dependency('glx')

# bx
bx_sources = files('bx/src/amalgamated.cpp')
bx_inc = include_directories('bx/include', 'bx/3rdparty')
bx = static_library('bx', bx_sources, dependencies: dl, include_directories: bx_inc)
bx_dep = declare_dependency(include_directories: bx_inc, link_with: bx)

# bimg 3rdparty
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
astc_dep = declare_dependency(include_directories: 'bimg/3rdparty/astc', link_with: astc)

astc_codec = static_library('astc-codec',
                            'bimg/3rdparty/astc-codec/src/decoder/astc_file.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/codec.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/endpoint_codec.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/footprint.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/integer_sequence_codec.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/intermediate_astc_block.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/logical_astc_block.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/partition.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/physical_astc_block.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/quantization.cc',
                            'bimg/3rdparty/astc-codec/src/decoder/weight_infill.cc',
                           include_directories: ['bimg/3rdparty', 'bimg/3rdparty/astc-codec', 'bimg/3rdparty/astc-codec/include'])
astc_codec_dep = declare_dependency(include_directories: ['bimg/3rdparty', 'bimg/3rdparty/astc-codec', 'bimg/3rdparty/astc-codec/include'], link_with: astc_codec)

iqa = static_library('iqa', 'bimg/3rdparty/iqa/source/convolve.c',
                    'bimg/3rdparty/iqa/source/decimate.c',
                    'bimg/3rdparty/iqa/source/math_utils.c',
                    'bimg/3rdparty/iqa/source/ms_ssim.c',
                    'bimg/3rdparty/iqa/source/mse.c',
                    'bimg/3rdparty/iqa/source/psnr.c',
                    'bimg/3rdparty/iqa/source/ssim.c',
                    include_directories: 'bimg/3rdparty/iqa/include')
iqa_dep = declare_dependency(include_directories: 'bimg/3rdparty/iqa/include', link_with: iqa)

libsquish = static_library('libsquish', 'bimg/3rdparty/libsquish/alpha.cpp')
libsquish_dep = declare_dependency(link_with: libsquish)

tinyexr = static_library('tinyexr', 'bimg/3rdparty/tinyexr/deps/miniz/miniz.c',
                        include_directories: ['bimg/3rdparty/tinyexr', 'bimg/3rdparty/tinyexr/deps/miniz'])
tinyexr_dep = declare_dependency(include_directories: ['bimg/3rdparty/tinyexr', 'bimg/3rdparty/tinyexr/deps/miniz'], link_with: tinyexr)

# bimg
bimg_sources = files('bimg/src/image_cubemap_filter.cpp',
                    'bimg/src/image_decode.cpp', 
                    'bimg/src/image_encode.cpp',
                    'bimg/src/image_gnf.cpp',
                    'bimg/src/image.cpp')
bimg_inc = include_directories('bimg/include', 'bimg/3rdparty')
bimg = static_library('bimg', bimg_sources, 
                    dependencies: [bx_dep, astc_dep, astc_codec_dep, iqa_dep, libsquish_dep, tinyexr_dep], 
                    include_directories: bimg_inc)
bimg_dep = declare_dependency(include_directories: bimg_inc, link_with: bimg)

#bgfx 3rdparty
glsl_optimizer_inc = include_directories('bgfx/3rdparty/glsl-optimizer/include',
                                        'bgfx/3rdparty/glsl-optimizer/src/mesa',
                                        'bgfx/3rdparty/glsl-optimizer/src/glsl',
                                        'bgfx/3rdparty/glsl-optimizer/src')

glcpp = static_library('glcpp', 'bgfx/3rdparty/glsl-optimizer/src/glsl/glcpp/glcpp-lex.c',
                      'bgfx/3rdparty/glsl-optimizer/src/glsl/glcpp/glcpp-parse.c',
                      'bgfx/3rdparty/glsl-optimizer/src/glsl/glcpp/pp.c',
                      include_directories: glsl_optimizer_inc)
mesa = static_library('mesa', 'bgfx/3rdparty/glsl-optimizer/src/mesa/program/prog_hash_table.c',
                     'bgfx/3rdparty/glsl-optimizer/src/mesa/program/symbol_table.c',
                     'bgfx/3rdparty/glsl-optimizer/src/mesa/main/imports.c',
                     include_directories: glsl_optimizer_inc)

glsl_optimizer = static_library('glsl-optimizer', 'bgfx/3rdparty/glsl-optimizer/src/glsl/ast_array_index.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ast_expr.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ast_function.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ast_to_hir.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ast_type.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/builtin_functions.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/builtin_types.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/builtin_variables.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_lexer.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_optimizer.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_parser.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_parser_extras.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_symbol_table.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/glsl_types.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/hir_field_selection.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_basic_block.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_builder.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_clone.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_constant_expression.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_equals.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_expression_flattening.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_function_can_inline.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_function.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_function_detect_recursion.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_hierarchical_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_hv_accept.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_import_prototypes.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_print_glsl_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_print_metal_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_print_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_rvalue_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_stats.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_unused_structs.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_validate.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/ir_variable_refcount.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_atomics.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/linker.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_functions.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_interface_blocks.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_uniform_block_active_visitor.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_uniform_blocks.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_uniform_initializers.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_uniforms.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/link_varyings.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/loop_analysis.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/loop_controls.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/loop_unroll.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_clip_distance.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_discard.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_discard_flow.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_if_to_cond_assign.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_instructions.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_jumps.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_mat_op_to_vec.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_named_interface_blocks.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_noise.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_offset_array.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_output_reads.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_packed_varyings.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_packing_builtins.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_ubo_reference.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_variable_index_to_cond_assign.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_vec_index_to_cond_assign.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_vec_index_to_swizzle.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_vector.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_vector_insert.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/lower_vertex_id.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_algebraic.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_array_splitting.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_constant_folding.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_constant_propagation.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_constant_variable.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_copy_propagation.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_copy_propagation_elements.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_cse.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_dead_builtin_variables.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_dead_builtin_varyings.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_dead_code.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_dead_code_local.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_dead_functions.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_flatten_nested_if_blocks.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_flip_matrices.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_function_inlining.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_if_simplification.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_minmax.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_noop_swizzle.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_rebalance_tree.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_redundant_jumps.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_structure_splitting.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_swizzle_swizzle.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_tree_grafting.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/opt_vectorize.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/s_expression.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/standalone_scaffolding.cpp',
                                'bgfx/3rdparty/glsl-optimizer/src/glsl/strtod.c',
                               include_directories: glsl_optimizer_inc,
                               link_with: [glcpp, mesa])
glsl_optimizer_dep = declare_dependency(include_directories: glsl_optimizer_inc, link_with: glsl_optimizer)

glslang = static_library('glslang', 'bgfx/3rdparty/glslang/glslang/GenericCodeGen/CodeGen.cpp',
                        'bgfx/3rdparty/glslang/glslang/GenericCodeGen/CodeGen.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/Constant.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/InfoSink.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/Initialize.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/IntermTraverse.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/Intermediate.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/ParseContextBase.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/ParseHelper.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/PoolAlloc.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/RemoveTree.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/Scan.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/ShaderLang.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/SpirvIntrinsics.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/SymbolTable.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/Versions.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/attribute.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/glslang_tab.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/intermOut.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/iomapper.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/limits.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/linkValidate.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/parseConst.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/propagateNoContraction.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/reflection.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/preprocessor/Pp.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/preprocessor/PpAtom.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/preprocessor/PpContext.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/preprocessor/PpScanner.cpp',
                        'bgfx/3rdparty/glslang/glslang/MachineIndependent/preprocessor/PpTokens.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslAttributes.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslGrammar.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslOpMap.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslParseables.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslParseHelper.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslScanContext.cpp',
                        'bgfx/3rdparty/glslang/glslang/HLSL/hlslTokenStream.cpp',
                        'bgfx/3rdparty/glslang/hlsl/stub.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/CInterface/spirv_c_interface.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/disassemble.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/doc.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/GlslangToSpv.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/InReadableOrder.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/Logger.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/SpvBuilder.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/SpvPostProcess.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/SPVRemapper.cpp',
                        'bgfx/3rdparty/glslang/SPIRV/SpvTools.cpp',
                        'bgfx/3rdparty/glslang/OGLCompilersDLL/InitializeDll.cpp',
                        'bgfx/3rdparty/glslang/glslang/OSDependent/Unix/ossource.cpp',
                        include_directories: ['bgfx/3rdparty', 'bgfx/3rdparty/spirv-tools/include', 'bgfx/3rdparty/spirv-tools/source', 'bgfx/3rdparty/glslang', 'bgfx/3rdparty/glslang/glslang/Include', 'bgfx/3rdparty/glslang/glslang/Public'])
glslang_dep = declare_dependency(include_directories: ['bgfx/3rdparty/spirv-tools/include', 'bgfx/3rdparty/spirv-tools/source', 'bgfx/3rdparty/glslang', 'bgfx/3rdparty/glslang/glslang/Include', 'bgfx/3rdparty/glslang/glslang/Public'], link_with: glslang)

spirv_cross = static_library('spirv-cross', 'bgfx/3rdparty/spirv-cross/main.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cfg.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cpp.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cross.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cross_c.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cross_parsed_ir.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_cross_util.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_glsl.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_hlsl.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_msl.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_parser.cpp',
                        'bgfx/3rdparty/spirv-cross/spirv_reflect.cpp',
                        include_directories: ['bgfx/3rdparty/spirv-cross', 'bgfx/3rdparty/spirv-cross/include'])
spirv_cross_dep = declare_dependency(include_directories: ['bgfx/3rdparty/spirv-cross', 'bgfx/3rdparty/spirv-cross/include'], link_with: spirv_cross)

webgpu_dep = declare_dependency(include_directories: 'bgfx/3rdparty/webgpu/include')

# bgfx
bgfx_sources = files('bgfx/src/amalgamated.cpp')
bgfx_inc = include_directories('bgfx/include', 'bgfx/3rdparty', 'bgfx/3rdparty/dxsdk', 'bgfx/3rdparty/khronos')

bgfx = library('bgfx', bgfx_sources, include_directories: bgfx_inc, dependencies: [threads, x11, glx, bx_dep, bimg_dep])
bgfx_dep = declare_dependency(include_directories: bgfx_inc, link_with: bgfx)

# bgfx tools
shaderc = executable('shaderc', 'bgfx/tools/shaderc/shaderc_glsl.cpp',
                    'bgfx/tools/shaderc/shaderc_hlsl.cpp',
                    'bgfx/tools/shaderc/shaderc_metal.cpp',
                    'bgfx/tools/shaderc/shaderc_pssl.cpp',
                    'bgfx/tools/shaderc/shaderc_spirv.cpp', 
                    dependencies: [threads, bx_dep, bgfx_dep, glsl_optimizer_dep, glslang_dep, spirv_cross_dep, webgpu_dep])

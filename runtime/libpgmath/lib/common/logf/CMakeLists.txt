
#
# Copyright (c) 2019, NVIDIA CORPORATION.  All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

get_property(FLAGS GLOBAL PROPERTY "FLAGS_X8664_L1")
get_property(DEFINITIONS GLOBAL PROPERTY "DEFINITIONS_X8664_L1")


set(SRCS_SCALAR
  fs_log_scalar.cpp
  )

set(SRCS_VECTOR
  fs_log_vector.cpp
  )

list(APPEND DEFINITIONS NDEBUG)
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  list(APPEND DEFINITIONS _GNU_SOURCE)
endif()

# Scalar 
set(FLAGS_TMP "${FLAGS} -mtune=core-avx2 -march=core-avx2 -D_CPU=avx2")
libmath_add_object_library("${SRCS_SCALAR}" "${FLAGS_TMP}" "${DEFINITIONS}" "logf-avx2_1")


# Vector, Four elements
set(FLAGS_TMP "${FLAGS} -mtune=core-avx2 -march=core-avx2 -D_CPU=avx2 -D_VL=4")
libmath_add_object_library("${SRCS_VECTOR}" "${FLAGS_TMP}" "${DEFINITIONS}" "logf-avx2_4")


# Vector, Eight elements
set(FLAGS_TMP "${FLAGS} -mtune=core-avx2 -march=core-avx2 -D_CPU=avx2 -D_VL=8")
libmath_add_object_library("${SRCS_VECTOR}" "${FLAGS_TMP}" "${DEFINITIONS}" "logf-avx2_8")


# Vector, Sixteen elements
set(FLAGS_TMP "${FLAGS} -mtune=skylake-avx512 -march=skylake-avx512 -D_CPU=avx512 -D_VL=16")
libmath_add_object_library("${SRCS_VECTOR}" "${FLAGS_TMP}" "${DEFINITIONS}" "logf-avx512_16")

set(FLAGS_TMP "${FLAGS} -mtune=knl -march=knl -D_CPU=knl -D_VL=16")
libmath_add_object_library("${SRCS_VECTOR}" "${FLAGS_TMP}" "${DEFINITIONS}" "logf-knl_16")


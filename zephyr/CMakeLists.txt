# -------------------------------------------------------------------------------
# Copyright (c) 2022-2023, Arm Limited. All rights reserved.
#
# SPDX-License-Identifier: BSD-3-Clause
#
# See BSD-3-Clause license in README.md
# -------------------------------------------------------------------------------

if(CONFIG_QCBOR)
    # Configuration:
    # Floating-point support (see README.md for more information)
    set(QCBOR_OPT_DISABLE_FLOAT_HW_USE ON CACHE BOOL "Eliminate dependency on FP hardware and FP instructions")
    set(QCBOR_OPT_DISABLE_FLOAT_PREFERRED OFF CACHE BOOL "Eliminate support for half-precision and CBOR preferred serialization")
    set(QCBOR_OPT_DISABLE_FLOAT_ALL OFF CACHE BOOL "Eliminate floating-point support completely")

    zephyr_include_directories(
        ${ZEPHYR_QCBOR_MODULE_DIR}/inc
    )

    zephyr_library()
    zephyr_library_sources(
        ${ZEPHYR_QCBOR_MODULE_DIR}/src/ieee754.c
        ${ZEPHYR_QCBOR_MODULE_DIR}/src/qcbor_decode.c
        ${ZEPHYR_QCBOR_MODULE_DIR}/src/qcbor_encode.c
        ${ZEPHYR_QCBOR_MODULE_DIR}/src/qcbor_err_to_str.c
        ${ZEPHYR_QCBOR_MODULE_DIR}/src/UsefulBuf.c
    )

    zephyr_compile_definitions_ifdef(
        QCBOR_OPT_DISABLE_FLOAT_HW_USE
        QCBOR_DISABLE_FLOAT_HW_USE
    )

    zephyr_compile_definitions_ifdef(
        QCBOR_OPT_DISABLE_FLOAT_PREFERRED
        QCBOR_DISABLE_PREFERRED_FLOAT
    )

    zephyr_compile_definitions_ifdef(
        QCBOR_OPT_DISABLE_FLOAT_ALL
        USEFULBUF_DISABLE_ALL_FLOAT
    )
endif()

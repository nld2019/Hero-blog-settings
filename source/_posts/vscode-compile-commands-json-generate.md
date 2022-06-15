---
title: vscode-compile_commands.json generate
date: 2022-06-15 22:33:32
tags: 'Misc'
---

compile_commands.json can be used for vscode to prase the code and corresponding code branch.
============================================================================================

1. generate compile_commands.json directly by bear command.

    make
    bear make -nB


2. generate compile_commands.json by compiledb

    make
    copiledb make -nB

3. generate compile_commands.json by build log

    compiledb < build-log.txt


After generate the compile_commands.json successfully, move the file to source code path and add it to vscode settings.
Sometimes, vscode may not prase the file correctly. may need do minor changes for the file.

## An example of compile_commands.json

    {
            "directory": "/home/nield/scode/FW/fw_v17_dev/src/wlan/src/wifidirect",
            "arguments": [
                    "armcc",
            "--thumb",
            "-M",
            "--no_depend_system_headers",
            "--cpu",
            "Cortex-R7.no_vfp",
            "--dwarf3",
            "-g",
            "-O2",
            "--c99",
            "--split_sections",
            "-DBUILD_WLAN",
            "-DOSA_THREADX",
            "-DEEPROM_ACCESS",
            "-DBTCOEX_VER_2",
            "-DBTCOEX_SPATIAL",
            "-DRBC_SIMPLIFIED_BCA_TDM_COEX_MODE",
            "-DIEEE_PSPOLL",
            "-DDOT11H",
            "-DDOT11H_TPC",
            "-DDFS_RADAR_DETECT",
            "-DJAPAN_W53_NEW_PATTERN",
            "-DDOT11K",
            "-DDOT11K_VE",
            "-DCHANRPT",
            "-DUNII_4_SUPPORT",
            "-DDOT11W",
            "-DENABLE_GCMP_SUPPORT",
            "-DSSU_SUPPORT",
            "-DDOT11P",
            "-DASSOC_AGENT_MLME",
            "-DSCAN_AGENT_MLME",
            "-DENHANCED_POWER_SAVE",
            "-DSEND_ASSOC_REQ_IE_TO_HOST",
            "-I/home/nield/scode/FW/fw_v17_dev/src/build/../common/modules/OS/ThreadX",
            "-I/home/nield/scode/FW/fw_v17_dev/src/build/../btu2/rom/modules/bt/incl",
            "-I/home/nield/scode/FW/fw_v17_dev/src/build/../btu2/rom/modules/bt/btds/incl",
            "-I/home/nield/scode/FW/fw_v17_dev/src/build/../btu2/rom/modules/btble/incl"
            ],
            "file": ["wifiDirect_services.c",
                    "wifiDirect_main.c",
                    "SyncWifiDirect_srv.c",
                    "wifiDirect_ps.c"
            ]
    
    },



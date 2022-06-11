---
title: 'CODE-ANALYSIS:1-module-init-process'
date: 2022-06-11 23:37:12
tags: code analysis, Wi-Fi, Driver;
---

2022/06/03

// driver code enter point
module_init(woal_init_module);


init card adapter card, maximum support is 4


init proc and crate proc/mwlan  wifi_status


init FW hang work queue.

init hang work to process FW hang

init register work queue

init register work to process bus driver register.
queue_work register work to register work queue.


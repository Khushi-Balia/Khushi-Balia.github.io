---
layout: post
title: Weekly meets and targets
date: 2023-06-01 15:53:00-0400
description: Bi-weekly meets 
categories: 
giscus_comments: 
related_posts: 
toc:
  sidebar: left
---

This blog will be updates Bi-weekly.

#### WEEK 1 and 2

##### MOM - 27th May, 2023


- Discussed the scope and implementation of the project

- Set targets for Week 1
1. Go through the PRU Assembly, compile some examples using pru-gcc
2. Refer the Cpu0 backend, going through the codebase and implementation
3. Read the PRU section of the reference manual, and understand more about it

- Suggestion given : Look for a way to take care of the special functions in the language, like gpio pins configuration, debugging functionality and so on

- Final goal : Upstreaming the backend

- References shared:
  [https://mythopoeic.org/BBB-PRU/am335xPruReferenceGuide.pdf](https://mythopoeic.org/BBB-PRU/am335xPruReferenceGuide.pdf)
  [https://www.ti.com/lit/ug/spruh73q/spruh73q.pdf](https://www.ti.com/lit/ug/spruh73q/spruh73q.pdf)


#### WEEK 3 and 4

##### MOM - 10th June, 2023

- Discussed some PRU instructions and registers

- Set target for Week 2
1. Modify the existing implementation of PRU Backend to support the latest LLVM version 16
2. Write the registerinfo.td for PRU
3. Write the instrinfo.td for PRU

- References shared:
  [https://llvm.org/docs/TableGen/ProgRef.html](https://llvm.org/docs/TableGen/ProgRef.html)
  [https://www.ti.com/lit/SPRUIJ2](https://www.ti.com/lit/SPRUIJ2)
  [https://mythopoeic.org/BBB-PRU/am335xPruReferenceGuide.pdf](https://mythopoeic.org/BBB-PRU/am335xPruReferenceGuide.pdf)
  [https://gcc.gnu.org/legacy-ml/gcc-patches/2018-06/msg00775.html](https://gcc.gnu.org/legacy-ml/gcc-patches/2018-06/msg00775.html)
  [https://github.com/bryant/llvm-pru](https://github.com/bryant/llvm-pru)



<center>
    <img width="100" src="https://raw.githubusercontent.com/CyberReasoningSystem/wiki/main/logo/export.png"/>
    <h1>OpenCRS</h1>
</center>

## About

OpenCRS is an open-source [cyber reasoning system](https://www.trailofbits.com/services/published-research/cyber-reasoning-system-crs) capable of detecting, exploiting, and patching vulnerabilities in `i386` ELF executables, built from C codebases.

## Repositories

### CRS Modules

- **[`dataset`](https://github.com/CyberReasoningSystem/dataset)** for compiling and managing vulnerable programs
- **[`attack_surface_approximation`](https://github.com/CyberReasoningSystem/attack_surface_approximation)** for discovering the attack surface in an executable
- **[`vulnerability_detection`](https://github.com/CyberReasoningSystem/vulnerability_detection)** for finding vulnerabilities in executables
- **[`vulnerability_analytics`](https://github.com/CyberReasoningSystem/vulnerability_analytics)** for analyzing found vulnerabilities to extract more information (for example, the root cause)
- **[`automatic_exploit_generation`](https://github.com/CyberReasoningSystem/automatic_exploit_generation)** for automatically generating exploits

### Helpers

- **[`opencrs_dataset`](https://github.com/CyberReasoningSystem/opencrs_dataset)** for storing 54k vulnerable ELF executables
- **[`nist_c_test_suite`](https://github.com/CyberReasoningSystem/nist_c_test_suite)** for storing NIST's "C Test Suite for Source Code Analyzer v2 - Vulnerable" dataset
- **[`vagrant_infra`](https://github.com/CyberReasoningSystem/vagrant_infra)** for creating VMs with OpenCRS's modules
- **[`commons`](https://github.com/CyberReasoningSystem/commons)**, with utility functions and classes, enums, and interfaces that are used in multiple CRS modules
- **[`zeratool_lib`](https://github.com/CyberReasoningSystem/zeratool_lib)**, a fork of Zeratool for migrating the CLI tool into a Python 3 library for exploiting executables on the local machine

### Meta

- **[`wiki`](https://github.com/CyberReasoningSystem/wiki)** as a non-functional, meta-repository for describing how OpenCRS works as an organization and storing miscellaneous information
- **[`awesome-binary-analysis`](https://github.com/CyberReasoningSystem/awesome-binary-analysis)** for helpful binary analysis tools and research materials

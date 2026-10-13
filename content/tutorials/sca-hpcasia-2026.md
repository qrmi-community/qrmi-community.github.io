---
title: Workload and workflow management in quantum-classical HPC
event: SCA/HPCAsia 2026 Tutorial
description: SCA/HPCAsia 2026 tutorial on running quantum workloads with Slurm and orchestrating hybrid quantum-classical workflows with Prefect.
date: 2026-01-26
when: Monday, January 26, 2026, 1:30pm – 4:30pm
room: "1007"
city: Osaka, Japan
materials: https://github.com/qrmi-community/SCA-HPCAsia-2026
modules:
  - name: Towards the Integration of HPC and Quantum Computing
    detail: Munetaka Ohtani
  - name: Workflow and Workload Management in Quantum-Classical HPC
    detail: Shweta Salaria
presenters:
  - name: Munetaka Ohtani
    affiliation: IBM Quantum
    photo: images/presenters/munetaka-ohtani.jpg
  - name: Yoonho Park
    affiliation: IBM Research
    photo: images/presenters/yoonho-park.jpg
  - name: Shweta Salaria
    affiliation: IBM Research
---

Integrating quantum processing units (QPUs) into heterogeneous HPC
infrastructure, the quantum-centric supercomputing model, requires
collaboration across industries to align hardware and software.

The first half covered the architecture IBM and its HPC and quantum partners
built to run QPU workloads under the Slurm workload manager, with live
demonstrations of Slurm jobs running quantum and quantum-classical hybrid
workloads.

The second half addressed orchestration challenges specific to hybrid
workloads, such as iterative execution, hyperparameter tuning and backend
instability, and showed how to build fault-tolerant pipelines with Prefect
using checkpointing, automatic retries and real-time observability.

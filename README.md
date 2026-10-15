# AHB-to-APB Bridge — Learning and RTL Design

My goal is to learn how to design and verify an AHB-Lite-to-APB bridge.

This project will cover bus protocols, timing diagrams, state machines,
RTL design and verification.

Current progress: Day 5 handwritten notes prepared and reviewed. Protocol
study is in progress; bridge RTL implementation and simulation have not started.

| Session | Study and recorded artifacts |
| --- | --- |
| Day 1 | AMBA overview and bridge purpose; four handwritten pages |
| Day 2 | AHB basic transfers, pipelining and interconnection introduction; five pages |
| Day 3 | Legacy AHB interconnection, bursts and WRAP4 calculations; six pages and review corrections |
| Day 4 | Legacy AHB SPLIT transfers and bus data width through class page 29; three pages and review corrections |
| Day 5 | APB states and introductory read/write timing; six pages and review corrections |

The journals record corrections alongside the original handwritten photos.
Outstanding work includes revising those annotations, drawing an APB write
with a wait cycle, and finishing the earlier numeric AHB two-write exercise.

Planned bridge scope: single-clock, 32-bit AHB-Lite-to-APB3, aligned word
reads/writes, one active transaction, peripheral waits and errors. Legacy
full-AHB SPLIT/RETRY and arbitration are background study only.

- notes/: handwritten notes and diagrams.
- journal/: daily learning progress.
## Study checkpoint — 15 October 2026

Limited time today, so this update records my current checkpoint.
Next: revise the APB state diagram to include PREADY and draw a write
transfer with one wait cycle, marking the exact completion edge.

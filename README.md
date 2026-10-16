# AHB-Lite-to-APB3 Bridge — Learning and RTL Design

This repository documents my progress towards understanding,
designing and verifying an AHB-Lite-to-APB3 bridge.

It contains handwritten notes, daily journals, learning resources
and, later, the bridge implementation and verification results.

## Current status

Day 6 AHB fundamentals notes and journal are published.

Protocol study is in progress. Bridge RTL implementation and
simulation have not started.

## Learning progress

| Session | Work recorded |
| --- | --- |
| Day 1 | AMBA overview and bridge purpose; four handwritten pages |
| Day 2 | AHB basic transfers, pipelining and introductory interconnection; five pages |
| Day 3 | Legacy AHB interconnection, bursts and WRAP4 calculations; six pages |
| Day 4 | Legacy AHB SPLIT transfers and bus data width; three pages |
| Day 5 | APB states and introductory read/write timing; six pages |
| Day 6 | Supplementary AHB basics lesson, master-interface diagram and signal terminology; three pages |

The institute’s AMBA material has been covered. Supplementary
video lessons are being used to strengthen conceptual understanding,
alongside the official Arm specifications.

## Repository structure

- `notes/` — handwritten study notes and diagrams.
- `journal/` — daily work summaries and links to resources studied.

## Resources

- Institute-provided AMBA Bus Architecture material.
- Arm AHB specification: IHI0033C.
- Arm APB specification: IHI0024D.
- [Day 6 supplementary video lesson](https://www.youtube.com/watch?v=dxKE1wHvHCg)

The official Arm specifications are the reference for protocol behavior.

## Planned implementation scope

- Single-clock, 32-bit AHB-Lite-to-APB3 bridge.
- Aligned word reads and writes.
- One active transaction at a time.
- APB wait-state handling.
- APB error propagation using the required AHB-Lite error response.

Legacy full-AHB arbitration, SPLIT and RETRY are background
study topics and are outside the baseline implementation.

## Next steps

- Continue supplementary lessons and protocol exercises.
- Practise transfer timing, wait states and address/data pairing.
- Define the bridge requirements and architecture.
- Implement and verify the RTL after consolidating the fundamentals.

## Study checkpoint — 15 October 2026

Limited time was available, so this session recorded the current
checkpoint and the next APB timing exercise.

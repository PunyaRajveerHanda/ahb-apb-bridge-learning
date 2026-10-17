# AHB-Lite-to-APB3 Bridge — Learning and RTL Design

This repository documents my progress towards understanding,
designing and verifying an AHB-Lite-to-APB3 bridge.

It contains handwritten notes, daily journals, learning resources
and, later, the bridge implementation and verification results.

## Current status

Day 7 AHB basic transfers and wait-state timing notes and journal are published.

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
| Day 7 | AHB address/data phases, pipelined transfers, HREADY and wait-state timing; three pages |

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
- [Day 7 supplementary video — AHB transfer concepts](https://youtu.be/3SFTy4on9sc)
- [Day 7 supplementary video — AHB transfer timing and wait states](https://youtu.be/-EbEzAacqoQ)

The supplementary videos are from the same playlist being followed
from Day 6 onward. The official Arm specifications remain the
reference for protocol behavior.

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
- Strengthen understanding of AHB pipelined address and data phases.
- Practise transfer timing, wait states and address/data pairing.
- Relate AHB transfer completion to the APB side of the bridge.
- Define the bridge requirements and architecture.
- Implement and verify the RTL after consolidating the fundamentals.

## Study checkpoint — 17 October 2026

Day 7 focused on AHB basic transfer timing. Address and data phases,
pipelining, HREADY-controlled transfer completion and wait-state
behavior were studied using handwritten timing diagrams.

Examples included a basic transfer, a read transfer with two wait
states and a write transfer with one wait state. The next focus is
to continue strengthening transfer timing and connect these AHB
concepts to the APB side of the bridge.

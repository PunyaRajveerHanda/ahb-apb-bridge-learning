# AHB-Lite-to-APB3 Bridge — Learning and RTL Design

This repository documents my progress towards understanding,
designing and verifying an AHB-Lite-to-APB3 bridge.

It contains handwritten notes, daily journals, learning resources
and, later, the bridge implementation and verification results.

## Current status

Study progress has reached Day 10: WRAP8 read-transfer timing,
wrap-boundary calculations and undefined-length INCR bursts.

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
| Day 8 | AHB overlapping transfers, wait states, HTRANS types and HSIZE encodings; three pages |
| Day 9 | HBURST encodings, INCR4/WRAP4 address sequences, wrap-block calculations, alignment and burst timing; four pages |
| Day 10 | WRAP8 read timing, 32-byte wrap-block calculation and undefined-length INCR operation; three pages |

The institute’s AMBA material has been covered. Supplementary
video lessons are being used to strengthen conceptual understanding,
alongside the official Arm specifications.

## Repository structure

- `notes/` — handwritten study notes and diagrams.
- `journal/` — daily work summaries and links to resources studied.

## Resources

- Institute-provided AMBA Bus Architecture material.
- [Arm AHB specification — IHI0033C](https://developer.arm.com/documentation/ihi0033/c/)
- Arm APB specification: IHI0024D.
- [Day 6 supplementary video lesson](https://www.youtube.com/watch?v=dxKE1wHvHCg)
- [Day 7 supplementary video — AHB transfer concepts](https://youtu.be/3SFTy4on9sc)
- [Day 7 supplementary video — AHB transfer timing and wait states](https://youtu.be/-EbEzAacqoQ)
- [Day 8 supplementary lesson 1](https://youtu.be/M4CxmSdoSzA)
- [Day 8 supplementary lesson 2](https://youtu.be/v2EcFDnWIrw)
- [Day 9 supplementary lesson](https://youtu.be/1or91qL6bds)
- [Day 10 supplementary lesson](https://youtu.be/4Wu0p0nN0Wo)

The supplementary videos are from the playlist being followed
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

Burst-protocol study does not mean that burst support has been
implemented. The bridge’s handling of incoming burst beats will
be specified before RTL development.

## Next steps

- Continue supplementary lessons and protocol exercises.
- Practise fixed-length and undefined-length burst examples.
- Strengthen transfer timing, wait-state and address/data pairing concepts.
- Relate AHB transfer completion to APB transactions.
- Define the bridge requirements and architecture.
- Implement and verify the RTL after consolidating the fundamentals.

## Latest study checkpoint — 20 October 2026

Day 10 focused on WRAP8 and undefined-length INCR bursts.

Three handwritten pages and the [Day 10 journal](journal/day-010.md)
record a WRAP8 word-read example starting at `0x34`, its 32-byte
wrap block, read-transfer timing and the operation of INCR bursts.

The main takeaway is the distinction between a fixed wrapping
burst and an incrementing burst without a predetermined beat count.
Further study will connect these concepts to the bridge architecture.

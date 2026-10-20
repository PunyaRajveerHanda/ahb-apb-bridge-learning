# AHB-to-APB Bridge — Learning and RTL Design

This repository documents my progress towards understanding,
designing and verifying a full AHB-to-APB bridge, with APB3 peripheral interfaces.

It contains handwritten notes, daily journals, learning resources
and, later, the bridge implementation and verification results.

## Current status

Study progress has reached Day 10: WRAP8 read timing, wrap-boundary
calculations and undefined-length INCR bursts.

The implementation target is full AMBA 2 AHB, not AHB-Lite.
The APB side remains APB3 with peripheral wait and error handling.

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
- `docs/` — current implementation scope and learning route.

## Resources

- Institute-provided AMBA Bus Architecture material.
- [Full AHB: Arm AMBA 2 specification — IHI0011A](https://documentation-service.arm.com/static/642569a2314e245d086bc87e?token=)
- [Arm AHB Example AMBA System — architectural reference](https://documentation-service.arm.com/static/5f1061300daa596235e7b232?token=)
- [AHB-Lite/AHB5 comparison reference — IHI0033C](https://developer.arm.com/documentation/ihi0033/c/)
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

- Full AMBA 2 AHB slave to APB3 master bridge, with a shared clock and 32-bit address/data paths.
- Aligned word reads and writes as the first implementation milestone.
- Full-AHB two-bit HRESP interface; initially OKAY/ERROR responses and wait-state handling.
- APB SETUP/ACCESS sequencing, PREADY waits and PSLVERR propagation.
- SINGLE bring-up followed by explicitly verified beat-wise burst translation.
- A separate two-master arbitration/interconnect verification setup, including ownership, handover and locked sequences.
- A documented unsupported-request and reset policy before RTL development.

SPLIT/RETRY are part of full-AHB study and system-level reasoning. Generating
these responses in the bridge is a separately scoped extension; a full-AHB slave
need not implement every response mechanism. The arbiter remains outside the bridge.

No bridge RTL, simulations, burst support or multi-master integration are
claimed as complete. The current plan and verification gates are in
[the implementation route](docs/IMPLEMENTATION_PLAN.md).

Use the [protocol reference](docs/PROTOCOL_REFERENCE.md) alongside the original
handwritten notes for timing rules and the distinction between protocol versions.

## Next steps

- Continue supplementary lessons and complete the relevant full-AHB/APB3 reading.
- Practise burst, wait-state, ownership and handover timing examples.
- Define the bridge's exact interface and supported-feature contract.
- Build the driver, peripheral model and independent scoreboard.
- Implement and verify the bridge gradually, then the multi-master integration setup.
- Reconstruct the existing FIFO reference independently after the bridge milestone.

## Latest study checkpoint — 20 October 2026

Day 10 focused on WRAP8 read timing and undefined-length INCR bursts.
Three handwritten pages and the [Day 10 journal](journal/day-010.md) are published.
Today's study does not establish implemented burst support.

The next session will consolidate timing with independent exercises before
adding more lecture material. See the daily practice section in the implementation route.

On 20 October, the project direction changed from AHB-Lite to full AHB at my
request. The current route supersedes the old AHB-Lite implementation proposal;
earlier journals remain historical records of the plan at those dates.

# AHB-to-APB Bridge — Learning and RTL Design

This repository documents my progress towards understanding, designing
and verifying a full AMBA 2 AHB-to-APB bridge with APB3 peripheral interfaces.

It contains handwritten notes, daily journals, learning resources and
a staged implementation and verification plan.

## Current status

24 October 2026: AHB study is paused today due to other work.
The next study session is planned for 25 October, continuing the
AHB master example and tracing a SINGLE write with HREADY wait cycles.
No new lesson, notes or RTL results are recorded for today.

Study progress has reached Day 13: handwritten study of an AHB master
in Verilog, covering the module interface, internal registers, FIFO
signals, state logic and a word-sized SINGLE write sequence.
The master example has not yet been compiled or simulated.

The implementation target is full AHB. The APB side uses APB3,
including peripheral wait-state and error handling.

Protocol study is ongoing. Bridge RTL implementation and simulation
have not started.

## Learning progress

| Session | Work recorded |
| --- | --- |
| Day 1 | AMBA overview and bridge purpose; four handwritten pages |
| Day 2 | AHB basic transfers, pipelining and introductory interconnection; five pages |
| Day 3 | Legacy AHB interconnection, bursts and WRAP4 calculations; six pages |
| Day 4 | Legacy AHB SPLIT transfers and bus data width; three pages |
| Day 5 | APB states and introductory read/write timing; six pages |
| Day 6 | Supplementary AHB basics, master-interface diagram and signal terminology; three pages |
| Day 7 | AHB address/data phases, pipelined transfers, HREADY and wait-state timing; three pages |
| Day 8 | Overlapping transfers, wait states, HTRANS types and HSIZE encodings; three pages |
| Day 9 | HBURST encodings, INCR4/WRAP4 sequences, wrap-block calculations and alignment; four pages |
| Day 10 | WRAP8 read timing, 32-byte wrap-block calculation and undefined-length INCR operation; three pages |
| Day 11 | Slave responses, IDLE/BUSY wait-state examples and review of a supplied two-write timing diagram; three pages |
| Day 12 | Revision of earlier AHB concepts and introductory master-FSM study, including an interface sketch and read/write state notes; two pages |
| Day 13 | AHB master Verilog interface, internal registers, FIFO signals, state logic and SINGLE write sequence; [journal](journal/day-013.md) and five handwritten pages |

The institute's AMBA material has been covered. Supplementary video
lessons and official Arm specifications are being used to strengthen
understanding before implementation.

## Repository structure

- `notes/` — handwritten notes and supporting timing diagrams.
- `journal/` — daily study summaries, exercises and resource links.
- `docs/` — implementation scope, protocol reference and learning route.

## Day 11 worked example

Reviewed two independent SINGLE word writes without wait states:
- Write 0x11 to address 0x1000.
- Write 0x22 to address 0x1004.

The diagram illustrates address/data overlap and the completion edge
of each write. Both independent transfers begin with NONSEQ.

![AHB two-write timing example](notes/Q1.%20day%2011.jpeg)

This example was reviewed for understanding.
Independent reconstruction and a version with one wait state are
planned for the next session.

See the [Day 11 journal](journal/day-011.md) for the complete question
and study summary.

## Planned implementation scope

- Full AMBA 2 AHB slave to APB3 master bridge.
- Shared clock and 32-bit address/data paths.
- Aligned word SINGLE reads and writes for initial bring-up.
- Two-bit HRESP interface with initial OKAY/ERROR response handling.
- APB SETUP/ACCESS sequencing and PREADY wait states.
- PSLVERR handling with the appropriate AHB error response.
- Beat-wise burst translation after SINGLE transfers are verified.
- Separate two-master arbitration and interconnect verification.
- Ownership, handover and locked-sequence testing.
- Defined reset, address-map and unsupported-request behavior.

SPLIT and RETRY remain part of full-AHB study.
Generating these responses in the bridge is a separately scoped
extension. The arbiter remains outside the bridge.

Burst support, multi-master integration and bridge verification
are planned work and are not claimed as complete.

See the [implementation route](docs/IMPLEMENTATION_PLAN.md) and
[protocol reference](docs/PROTOCOL_REFERENCE.md).

## Resources

- Institute-provided AMBA Bus Architecture material.
- [Full AHB: Arm AMBA 2 specification — IHI0011A](https://documentation-service.arm.com/static/642569a2314e245d086bc87e?token=)
- [Arm AHB Example AMBA System — architectural reference](https://documentation-service.arm.com/static/5f1061300daa596235e7b232?token=)
- [AHB-Lite/AHB5 comparison reference — IHI0033C](https://developer.arm.com/documentation/ihi0033/c/)
- Arm APB specification — IHI0024D.
- [Day 6 supplementary lesson](https://youtu.be/dxKE1wHvHCg)
- [Day 7 lesson: transfer concepts](https://youtu.be/3SFTy4on9sc)
- [Day 7 lesson: timing and wait states](https://youtu.be/-EbEzAacqoQ)
- [Day 8 supplementary lesson 1](https://youtu.be/M4CxmSdoSzA)
- [Day 8 supplementary lesson 2](https://youtu.be/v2EcFDnWIrw)
- [Day 9 supplementary lesson](https://youtu.be/1or91qL6bds)
- [Day 10 supplementary lesson](https://youtu.be/4Wu0p0nN0Wo)
- [Day 11 supplementary lesson](https://youtu.be/39QRExXRBSs)
- [Day 12 supplementary lesson — continued study and revision](https://youtu.be/39QRExXRBSs)
- [Day 13 supplementary lesson — AHB master Verilog study](https://youtu.be/uzEg7ziaSaE)

The official specifications remain the reference for protocol behavior.
Differences between full AHB and simplified interfaces are considered
when applying lesson material to the project.

## Next steps

- Independently reconstruct the reviewed two-write timing example.
- Extend it with one wait state and explain the completion edges.
- Develop the master example with explicit state widths, FIFO bounds and wait-state handling.
- Compile and simulate the SINGLE transfer path before extending it.
- Continue full-AHB and APB3 protocol study.
- Define the bridge's exact supported-feature contract.
- Build the driver, peripheral model and independent scoreboard.
- Implement and verify the bridge incrementally.
- Add the separate multi-master integration setup.
- Reconstruct the existing FIFO reference independently after the bridge milestone.

## Latest study checkpoint — 23 October 2026

Day 13 focused on the Verilog structure of an AHB master, including
its module interface, internal storage, FIFO signals and FSM logic.
The handwritten example develops a SINGLE word-write sequence.

See the [Day 13 journal](journal/day-013.md) and handwritten notes:
[page 1](notes/day-013-01.jpg), [page 2](notes/day-013-02.jpg),
[page 3](notes/day-013-03.jpg), [page 4](notes/day-013-04.jpg),
[page 5](notes/day-013-05.jpg).

This is study of a partial example, not a compiled or simulated
master or bridge implementation. The next practice task is to trace
a SINGLE write with HREADY wait cycles before implementing and testing
that transfer path.

The current full-AHB-to-APB3 route supersedes the earlier AHB-Lite
proposal. Earlier journals remain historical records of the plan
at those dates.

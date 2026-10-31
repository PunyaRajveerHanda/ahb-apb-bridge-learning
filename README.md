# AHB-to-APB Bridge — Learning and RTL Design

This repository documents my progress towards understanding, designing and verifying a full AMBA 2 AHB-to-APB bridge with APB3 peripheral interfaces.

It contains handwritten notes, daily journals, learning resources and a staged implementation and verification plan.

## Current status

Study progress has reached **Day 18**: focused APB timing study covering write and read transfers, SETUP/ACCESS sequencing, `PENABLE`, `PREADY`, and transfers with and without wait states.

The AHB portion of the supplementary lecture series was completed on Day 15. A consolidated revision using the **official Arm AMBA/AHB specification material** still remains before moving deeper into implementation.

Focused APB protocol study is now progressing from interface fundamentals into actual transfer timing. The bridge targets APB3; `PPROT` and `PSTRB` are studied separately as APB4 additions.

Day 18 covered the minimum two-cycle APB transfer, the role of the slave-driven `PREADY` signal, and how the ACCESS phase is extended when a peripheral requires wait states.

The handwritten master/slave examples have not yet been compiled or simulated.

The implementation target is full AHB. The APB side uses APB3, including peripheral wait-state and error handling.

Protocol study is ongoing. Bridge RTL implementation and simulation have not started.

## Learning progress

| **Session** | **Work recorded** |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
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
| Day 13 | AHB master Verilog interface, internal registers, FIFO signals, state logic and SINGLE write sequence; [journal](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/journal/day-013.md) and five handwritten pages |
| Day 14 | INCR4 master extension, beat counting, FIFO/address progression, simple slave FSM and testbench stimulus; [journal](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/journal/day-014.md) and seven handwritten pages |
| Day 15 | Wrapping-burst block sizes, base-address masks, WRAP4 examples and draft Verilog extension; AHB lecture-series completion; [journal](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/journal/day-015.md) and three pages |
| Day 16 | Consolidated revision of the AMBA material covered so far using existing handwritten and study notes; no new RTL or simulation work |
| Day 17 | APB fundamentals, bridge context, request/response signals and master/slave diagrams; [journal](journal/day-017.md) and three handwritten pages |
| Day 18 | APB write/read transfers, SETUP and ACCESS sequencing, `PENABLE`, `PREADY`, and timing with and without wait states; [journal](journal/day-018.md) and three handwritten pages |

The institute's AMBA material has been covered. The supplementary AHB lecture series has also been completed. The official Arm specifications will now be used to consolidate and verify protocol understanding before implementation.

Focused APB study is continuing alongside this consolidation.

## Repository structure

- `notes/` — handwritten notes and supporting timing diagrams.
- `journal/` — daily study summaries, exercises and resource links.
- `docs/` — implementation scope, protocol reference and learning route.

## Day 11 worked example

Reviewed two independent SINGLE word writes without wait states:

- Write 0x11 to address 0x1000.
- Write 0x22 to address 0x1004.

The diagram illustrates address/data overlap and the completion edge of each write. Both independent transfers begin with NONSEQ.

[AHB two-write timing example](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/notes/Q1.%20day%2011.jpeg) ([image](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/raw/main/notes/Q1.%20day%2011.jpeg))

This example was reviewed for understanding. Independent reconstruction and a version with one wait state remain planned exercises.

See the [Day 11 journal](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/journal/day-011.md) for the complete question and study summary.

## Planned implementation scope

- Full AMBA 2 AHB slave to APB3 master bridge.
- Shared clock and 32-bit address/data paths.
- Aligned word SINGLE reads and writes for initial bring-up.
- Two-bit `HRESP` interface with initial OKAY/ERROR response handling.
- APB SETUP/ACCESS sequencing and `PREADY` wait states.
- `PSLVERR` handling with the appropriate AHB error response.
- Beat-wise burst translation after SINGLE transfers are verified.
- Separate two-master arbitration and interconnect verification.
- Ownership, handover and locked-sequence testing.
- Defined reset, address-map and unsupported-request behavior.

SPLIT and RETRY remain part of full-AHB study. Generating these responses in the bridge is a separately scoped extension. The arbiter remains outside the bridge.

Burst support, multi-master integration and bridge verification are planned work and are not claimed as complete.

See the [implementation route](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/docs/IMPLEMENTATION_PLAN.md) and [protocol reference](https://github.com/PunyaRajveerHanda/ahb-apb-bridge-learning/blob/main/docs/PROTOCOL_REFERENCE.md).

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
- [Day 14 supplementary lesson — INCR4, slave and testbench study](https://youtu.be/4jPAmDrMqfY)
- [Day 15 supplementary lesson — wrapping-burst address generation](https://youtu.be/3LSP1SvmoyA)
- [Day 17 supplementary lesson — APB fundamentals and signals](https://youtu.be/MIjXS7ap0Hk)
- [Day 18 supplementary lesson — APB read/write transfers and wait states](https://youtu.be/MThzJkZeVDI?si=1A2vykQqQBr5_WNo)

The official specifications remain the reference for protocol behavior. Differences between full AHB and simplified interfaces are considered when applying lesson material to the project.

## Next steps

- Revise and consolidate AHB understanding using the official Arm AMBA/AHB specification.
- Continue APB study with error response behavior and `PSLVERR`.
- Consolidate APB SETUP/ACCESS timing for both read and write transfers.
- Practice identifying the exact completion edge using `PSEL`, `PENABLE` and `PREADY`.
- Study how APB wait states will be represented inside the bridge FSM.
- Independently reconstruct the reviewed two-write AHB timing example.
- Extend the AHB example with one wait state and explain the completion edges.
- Develop the master example with explicit state widths, FIFO bounds and wait-state handling.
- Compile and simulate the SINGLE transfer path before extending it.
- Define the bridge's exact supported-feature contract.
- Build the driver, peripheral model and independent scoreboard.
- Implement and verify the bridge incrementally.
- Add the separate multi-master integration setup.
- Reconstruct the existing FIFO reference independently after the bridge milestone.

## Latest study checkpoint — Day 18, 29 October 2026

Continued focused APB study by moving from basic interface signals to complete read and write transfer timing.

Studied the two APB transfer phases:

```text
SETUP → ACCESS
```

During SETUP:

```text
PSEL = 1
PENABLE = 0
```

During ACCESS:

```text
PSEL = 1
PENABLE = 1
```

A transfer completes when the selected slave is ready during the ACCESS phase:

```text
PSEL = 1
PENABLE = 1
PREADY = 1
```

Studied both write and read transfers without wait states and with wait states.

For a write transfer, the master provides the address and write data through `PADDR` and `PWDATA`.

For a read transfer, the slave returns data through `PRDATA`.

The important wait-state behavior studied today was:

```text
PREADY = 0
```

during ACCESS means the slave is not yet ready to complete the transfer. The ACCESS phase is therefore extended while the address, control and write-data signals remain stable.

Once:

```text
PREADY = 1
```

the transfer can complete on the corresponding clock edge.

An important distinction reinforced today is that APB uses **SETUP and ACCESS phases**, rather than the pipelined address/data phase terminology used for AHB.

Also, `PREADY` is generated by the APB slave and is used by the master/bridge to determine when an ACCESS phase can complete.

See the [Day 18 journal](journal/day-018.md), [lecture](https://youtu.be/MThzJkZeVDI?si=1A2vykQqQBr5_WNo) and handwritten notes:

[page 1](notes/day-018-01.jpeg), [page 2](notes/day-018-02.jpeg), [page 3](notes/day-018-03.jpeg).

Next: continue APB protocol study with error handling and `PSLVERR`, then connect the APB timing knowledge to the control states required in the AHB-to-APB bridge FSM.

Official AHB specification consolidation remains pending; bridge RTL implementation and verification remain ahead.

The current full-AHB-to-APB3 route supersedes the earlier AHB-Lite proposal. Earlier journals remain historical records of the plan at those dates.

## Study checkpoint — 1 November 2026

I am continuing and finishing my APB protocol study. The next topics are APB error handling with PSLVERR and connecting the transfer timing to the full AHB-to-APB3 bridge control FSM. Protocol learning is still in progress; bridge RTL implementation and simulation have not started.


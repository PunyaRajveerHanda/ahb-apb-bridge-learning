# AHB-to-APB implementation and learning route

Current direction: 20 October 2026. This document supersedes the AHB-Lite
baseline in the older planner and handoff. It is a design plan, not implemented RTL.

## Target and references

Build a 32-bit, single-clock **full AMBA 2 AHB-to-APB bridge**. Retain **APB3**
as the selected peripheral interface, including PREADY and PSLVERR. The project
name is AHB-to-APB; APB3 is its precise interface version, not a change to AHB-Lite.

- Full AHB: [Arm AMBA 2, IHI0011A](https://documentation-service.arm.com/static/642569a2314e245d086bc87e?token=), Chapter 3.
- Architectural study: [Arm AHB Example AMBA System, DDI0170A](https://documentation-service.arm.com/static/5f1061300daa596235e7b232?token=), bridge and interconnect sections.
- APB3: use the APB3 behavior defined in the supplied IHI0024D document.
- IHI0033C remains useful for comparing AHB-Lite/AHB5; it does not define the
  legacy full-AHB arbitration and SPLIT interface being targeted here.

The Arm example uses an older APB interface. Its architecture is a study reference,
not an exact APB3 RTL replica. No third-party RTL repository has been selected for
replication, and no existing bridge RTL was found in this project to convert.
The student will derive and implement the design, then compare behavior with
the selected specifications. Preserve any reference code and its attribution.

## Module boundaries

The bridge is an **AHB slave and APB master**. It does not become an AHB master
and does not grant bus ownership. Keep the surrounding arbiter, address/control
multiplexer, write-data multiplexer and response selection separate from the bridge.

Plan a two-master integration harness with distinguishable transactions and APB
register-model peripherals. Begin bridge debugging with one driver, then require
the two-master handover tests before calling the integration milestone complete.
An arbiter may be a separately verified model initially; label testbench models
and synthesizable implementation separately. Do not count a model as built hardware.

## Proposed feature contract to freeze before RTL

| Area | Planned behavior |
|---|---|
| Clock and widths | Shared clock; 32-bit AHB address/data and APB data |
| Basic accesses | Aligned word reads/writes first; address map and unsupported-size/alignment error policy must be fixed before coding |
| Requests | Capture only a selected valid address phase when HREADY permits acceptance; retain its address, direction and applicable controls |
| Response interface | Full-AHB HRESP[1:0]: OKAY=00, ERROR=01, RETRY=10, SPLIT=11 |
| Initial bridge responses | Generate OKAY and correctly timed ERROR; use waits for APB latency |
| SPLIT/RETRY | Study full-system behavior; bridge generation is a separately specified extension, not implied or mandatory for every full-AHB slave |
| Burst traffic | Bring up SINGLE first, then translate accepted aligned word beats of INCR/INCR4/8/16 and WRAP4/8/16 into individual APB accesses |
| Beat addressing | Use the supplied AHB address for each accepted beat; do not invent a burst counter that substitutes for actual bus addresses |
| IDLE/BUSY | Do not issue APB accesses for these address phases; still finish an earlier valid transfer's data phase |
| APB timing | One SETUP phase, then ACCESS; hold the request stable through peripheral waits |
| Errors | Map completed APB errors and defined unsupported requests to the full-AHB ERROR encoding and response timing |
| Ownership | Surrounding logic retains address-phase and data-phase ownership correctly across stalls and handover |
| Locks | Include locked-ownership sequences in the integration tests; arbitration respects the chosen full-AHB lock contract |
| Reset | Specify acceptance, in-flight cancellation and completed APB side effects; no implicit rollback |

This is a constrained full-AHB slave design, not a claim that every optional
full-AHB feature is implemented. Freeze response timing, maximum-latency or
timeout assumptions, peripheral windows, protection behavior and all unsupported
traffic handling in a requirements document before coding. An arbitrary test
timeout is not a hardware timeout feature.

If SPLIT is later enabled, add the appropriate HMASTER/HSPLIT interface and
arbiter masking/release behavior, plus replay checks. Prevent duplicate APB side
effects on retries. RETRY/SPLIT are not interchangeable names for HREADY waits.
Do not add these features by only widening HRESP or renaming the project.

## Learning and build sequence

Day numbers continue from actual study progress. Days 1-10 remain valid; do not
restart them or rewrite their history. Continue one focused hour per session.
The larger full-AHB scope supersedes the old guaranteed-looking day allocation;
advance on understanding and evidence rather than a fixed finish date.

1. **Complete current protocol study.** Continue the selected lessons; revise
   address/data overlap, burst addressing, waits, errors, reset and APB timing.
2. **Read the correct full-AHB sections.** Cover arbitration, ownership/muxing,
   locked accesses, two-bit responses, SPLIT/RETRY and burst interruption.
   Solve handover examples before designing the integration logic.
3. **Finish APB3 understanding.** Independently explain SETUP/ACCESS, PREADY,
   read/write data timing and PSLVERR with drawn examples.
4. **Freeze the contract.** Write interface tables, address map, feature matrix,
   reset behavior, error policy, supported sizes/bursts and ownership rules.
5. **Build models and checks first.** AHB driver, configurable APB peripheral,
   independent expected transaction queue, monitors and completion checks.
6. **Implement the bridge gradually.** Address/control capture; write-data phase;
   SETUP/ACCESS; read response; waits; then correct ERROR handling and reset.
7. **Verify consecutive requests and bursts.** Check final-beat completion,
   BUSY insertion, wait stretching and exact address/data pairing without
   duplicate or dropped peripheral operations.
8. **Verify the full-AHB context.** Add two-master arbitration and separate muxes;
   test handover with distinct data, stalls, locks and interrupted bursts.
9. **Add selected advanced responses only if specified.** SPLIT/RETRY require
   their own contract and regression; otherwise document the bridge's use of
   OKAY/ERROR and waits without claiming SPLIT generation.
10. **Release with evidence.** Regression, lint, synthesis, selected waveforms,
    reproducible commands, source attribution and measured results. Timing or
    board claims need separate tools and evidence.
11. **Reconstruct the FIFO afterwards.** Preserve the existing FIFO reference
    release. Independently derive and rebuild it after the bridge milestone;
    this is a separate project, unaffected by changing the bridge protocol.

## Verification exit gates

- Every accepted valid AHB request matches exactly one intended APB access or a
  documented rejection; no peripheral access is caused by IDLE/BUSY.
- Read/write data belongs to the accepted request and correct bus owner.
- APB signals remain stable during waits; errors are sampled only at completion.
- Correct response encoding and multi-cycle non-OKAY timing are checked.
- Directed and reproducible randomized tests cover bursts, waits, reset,
  unsupported traffic, locks and multi-master handover as scoped.
- An independent scoreboard checks transactions; waveform appearance alone
  is insufficient. No coverage, performance or correctness claims before runs.

## Daily practice and consolidation

Use this starting allocation for a focused 60-minute session:

| Minutes | Work |
|---|---|
| 0-5 | Recall yesterday without notes; explain one signal or rule |
| 5-20 | A short lesson segment or relevant specification section |
| 20-40 | Solve one new address/timing exercise independently |
| 40-50 | Check against the specification and explain the completing edges |
| 50-55 | Record the useful rule/example and a remaining question |
| 55-60 | Photograph, write a factual journal and publish meaningful changes |

Pause longer lectures rather than using the entire session for transcription.
Revisit an older problem after a few sessions. Mark an exercise as independently
solved only after an attempt without the worked answer; record where help was
needed. A shorter busy-day session can be recall and one checkpoint without
pretending to have completed a lesson or implementation.

The next session should consolidate the first ten study sessions:

1. Draw two independent AHB word writes to 0x1000 and 0x1004, with data 0x11
   and 0x22, without waits. Pair every address with its data and completion edge.
2. Repeat with one wait on the first write; show the held signals and shifted
   completion edges. Explain address-phase and data-phase ownership separately.
3. Draw one APB3 write with an ACCESS wait, explicitly including PREADY, then
   mark the completing edge and explain why SETUP is still only one cycle.
4. Derive a fresh WRAP8 halfword address sequence from 0x16. State the block,
   increment, wrapping point and beat count. Attempt it before looking up an answer.

These are assigned exercises, not completed work. Do not create journal claims
or simulation results until the user has attempted them. After reviewing those
attempts, choose the next lecture or a small RTL exercise based on the gaps.
An initial APB controller or burst-address calculation exercise can be small;
the complete bridge should follow the frozen contract and verification gates.

## Scope history

- 20 October 2026: at the user's explicit request, the planned AHB side changed
  from AHB-Lite to full legacy AHB. APB3 waits/errors and the single-clock 32-bit
  starting point are retained. Historical journals and the v2 PDF record the
  earlier decision; this document is the current implementation route.

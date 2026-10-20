# Protocol reference alongside the handwritten notes

Updated 20 October 2026. Original photographs are retained. This reference
clarifies the protocol without changing what was studied on earlier dates.

## AHB timing and addressing

- Every valid burst beat has its own address phase. AHB does not send only the
  initial address and leave later addresses implicit.
- A data phase follows its corresponding address phase; it can span multiple
  cycles. Pipelining overlaps phases but does not guarantee a transfer per cycle.
- HREADY controls completion at the clock edge. A wait extends the current data
  phase and can extend the following address phase. Keep write data valid during
  the wait. IDLE/BUSY and non-OKAY responses have additional specified rules.
- Bytes per beat are 2**HSIZE. Word means four bytes, not the encoded value 010.
  WRAP8 means eight beats, not eight bytes. The wrap span is bytes per beat times
  the decoded beat count. Use transfer-size alignment and the aligned wrap block.
- INCR has no announced fixed beat count; it is not infinite. BUSY is a pause
  without a new data beat. IDLE or a new NONSEQ ends that burst.

## Full AHB and the scope change

The current target is legacy full AHB. HRESP[1:0] encodes OKAY=00, ERROR=01,
RETRY=10 and SPLIT=11. Do not use a reversed success/error interpretation.
AHB-Lite instead has a one-bit OKAY/ERROR response; specify the version whenever
interpreting a signal table. The bridge and surrounding arbiter have separate roles.

Earlier dated journals describe the AHB-Lite scope selected at that time. The
[current route](IMPLEMENTATION_PLAN.md) supersedes that implementation plan.

## APB3 timing

SETUP has PSEL=1 and PENABLE=0 for one cycle. ACCESS has both asserted and can
repeat while PREADY=0. Completion occurs at a rising edge with PSEL, PENABLE and
PREADY all HIGH. Keep the request stable through ACCESS waits. A following access
needs a new SETUP phase, even when the same peripheral remains selected.

## References

- [Full AHB, Arm IHI0011A](https://documentation-service.arm.com/static/642569a2314e245d086bc87e?token=)
- [APB, Arm IHI0024D](https://developer.arm.com/documentation/ihi0024/d/)
- [AHB-Lite/AHB5 comparison, Arm IHI0033C](https://developer.arm.com/documentation/ihi0033/c/)

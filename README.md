# UVM APB Memory Verification Project

## Project Description
Complete UVM verification environment for an AMBA APB v2.0 compliant memory slave (DUT). Implements driver, sequencer, monitor, scoreboard for read/write transactions. Includes sequences, functional coverage, protocol assertions, simulation scripts, waveform snapshots, and RTL defect fixes. Achieves 100% functional coverage and zero scoreboard errors post-fixes.

**Key Features:**
- APB protocol compliance (2-phase transfers, wait states via PREADY).
- Random/constrained sequences (write/read/read-after-write/corners).
- Scoreboard with reference memory model.
- Covergroups for addr/data/PWRITE/PREADY crosses.
- Assertions for phase stability, reset behavior.
- Parameters: ADDR_WIDTH=10 (1KB mem), DATA_WIDTH=32, RDY_COUNT=1 (1-cycle delay).

**Learning Outcomes:** UVM TB architecture, protocol verification, RTL debugging for VLSI students.

## DUT Overview
APB slave memory with clock/reset, supports writes (latches PWDATA) and reads (drives PRDATA). PREADY asserts after RDY_COUNT clocks.

**Original RTL Defects Fixed:**
1. Syntax: `& &` → `&`; `'hzzzz_zzzz` → `'z`.
2. PRDATA combo → registered to avoid glitches.
3. Read_enable timing: Sync to posedge for spec compliance.

RTL in `rtl/apb_mem.sv` (fixed).

## Setup & Simulation
### Requirements
- EDA: Questa/ModelSim 2023+, VCS (IEEE UVM 1.2+).
- Scripts: Makefile, run.do.

### Build & Run
```bash
make compile
make sim TEST=base_test     # Or write_test, read_test, rw_test
make view                    # GTKWave waveforms
make cov                     # Coverage report
```

**Example Output:**
UVM_INFO @ 0: Reporter [RNTST] UVM test done PASSED
Coverage: 98.5% lines, 100% toggles, 95% FSM.
No scoreboard mismatches.

**Waveform Snapshots:**
<img width="1591" height="586" alt="image" src="https://github.com/user-attachments/assets/fbd95abd-1c66-4034-ad82-74e170cf2cd9" />

## Test Scenarios
| Test | Description | Transactions | Expected |
|------|-------------|--------------|----------|
| base_test | Single write+read | 2 | Data match |
| write_test | 50 rand writes | 50 | No mem corrupt |
| read_test | 50 rand reads | 50 | Valid PRDATA |
| rw_test | Rand read-after-write | 100 | 100% match |

## Results
- **Defects Found/Fixed:** 3 RTL bugs (syntax, timing, z-handling).
- **Coverage:** 98%+ functional/protocol.
- **Passes:** All tests; assertions fire 0.

  

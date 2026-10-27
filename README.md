# Synchronous FIFO – UVM Verification

##  Overview


The main objective is to verify the functional correctness of the FIFO design using SystemVerilog and UVM, with emphasis on:

- FIFO data ordering
- Read/Write operations
- Full and Empty status flags
- Reset behavior
- Simultaneous Read/Write operations
- FIFO boundary conditions
- Pointer wrap-around
- Functional coverage
- Assertion-based verification
- Scoreboard-based reference model


### Parameters

| Parameter | Default | Description |
|---|---:|---|
| `DATA_WIDTH` | 8 | Number of bits per FIFO entry |
| `DEPTH` | 8 | Maximum number of entries stored |

### Interface

| Signal | Direction | Width | Description |
|---|---|---:|---|
| `clk` | Input | 1 | Sampling clock |
| `rst_n` | Input | 1 | Synchronous active-low reset |
| `wr_en` | Input | 1 | Write request |
| `rd_en` | Input | 1 | Read request |
| `din` | Input | `DATA_WIDTH` | Write data |
| `dout` | Output | `DATA_WIDTH` | Registered read data |
| `full` | Output | 1 | FIFO full flag |
| `empty` | Output | 1 | FIFO empty flag |

All functional operations occur on the rising edge of `clk`.

---

## FIFO Operation
### Reset Behavior
rst_n is synchronous to clock and active-low. When rst_n == 0 at a rising clock edge, reset has highest
priority and both read/write requests are ignored.

### Write Operation
On each rising edge of clk, when rst_n = 1, wr_en = 1 and full = 0, din is accepted and appended to
the FIFO. If full = 1, the write request is rejected and must not change the logical FIFO contents.

### Read Operation
On each rising edge of clk, when rst_n = 1, rd_en = 1 and empty = 0, the oldest valid FIFO entry is
removed and presented on dout. If empty = 1, the read request is rejected, dout clear to 0.


#  Test Scenarios

The UVM verification environment covers the following test scenarios:

| Test ID | Test Case | Initial Condition | Stimulus | Expected Behavior |
|---|---|---|---|---|
| **TC01** | Synchronous Reset | FIFO may contain valid data | Assert `rst_n = 0` and apply a rising clock edge | FIFO becomes empty, `empty = 1`, `full = 0`, `dout = 0`. Read/Write requests are ignored |
| **TC02** | Normal Write | FIFO is empty and not full | Set `wr_en = 1`, `rd_en = 0`, and apply valid `din` | Data is appended to the FIFO and `empty` becomes `0` |
| **TC03** | Fill FIFO to Full | FIFO is empty | Perform `DEPTH` valid write operations | All data is accepted in order. After the final write, `full = 1` and `empty = 0` |
| **TC04** | Write While Full | FIFO is full | Set `wr_en = 1`, `rd_en = 0`, and apply new `din` | Write request is rejected. FIFO contents remain unchanged and `full = 1` |
| **TC05** | Normal Read | FIFO contains valid data | Set `rd_en = 1`, `wr_en = 0` | Oldest FIFO entry is presented on `dout` and FIFO count decreases |
| **TC06** | Read While Empty | FIFO is empty | Set `rd_en = 1`, `wr_en = 0` | Read request is rejected, `empty = 1`, and `dout = 0` |
| **TC07** | Simultaneous Read/Write | FIFO is neither empty nor full | Set `wr_en = 1`, `rd_en = 1` with valid `din` | Both operations are accepted. Oldest data is read while new data is written |
| **TC08** | Read/Write While Full | FIFO is full | Set `wr_en = 1`, `rd_en = 1` with valid `din` | Read is accepted, write is rejected because FIFO was full before the clock edge. `full` becomes `0` |
| **TC09** | Read/Write While Empty | FIFO is empty | Set `wr_en = 1`, `rd_en = 1` with valid `din` | Write is accepted, read is rejected because FIFO was empty before the clock edge. `dout = 0` and `empty` becomes `0` |
| **TC10** | FIFO Ordering & Wrap-Around | FIFO is empty | Write and read multiple entries to force pointer wrap-around | All data must be returned in the exact order in which valid writes were accepted |


#

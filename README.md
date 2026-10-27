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
The verification environment covers the following test cases.

- TC01: Synchronous Reset 
- TC02: Normal Write 
- TC03: Fill FIFO to Full 
- TC04: Write While Full 
- TC05: Normal Read 
- TC06: Read While Empty 
- TC07: Simultaneous Read and Write 
- TC08: Simultaneous Read and Write While Full 
- TC09: Simultaneous Read and Write While Empty 
- TC10: FIFO Ordering and Wrap-Around 

These scenarios are defined in the FIFO functional verification specification. 

---

#

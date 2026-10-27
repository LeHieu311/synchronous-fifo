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


##  DUT Specification

The FIFO uses the following default configuration:
DATA_WIDTH [7:0] : Number of bits per FIFO entry
DEPTH [7:0] : Maximum number of entries stored

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

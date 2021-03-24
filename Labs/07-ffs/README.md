## 1. Preparation tasks
### D flip-flop
| **D** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | No change |
| 0 | 1 | 0 | Reset |
| 1 | 0 | 1 | No change |
| 1 | 1 | 1 | Set |

### JK flip-flop

q(n+1)=j./qn + /k . qn

| **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | 0 | No change |
| 0 | 0 | 1 | 1 | No change |
| 0 | 1 | 0 | 0 | Reset |
| 0 | 1 | 1 | 0 | Reset |
| 1 | 0 | 0 | 1 | Set |
| 1 | 0 | 1 | 1 | Set |
| 1 | 1 | 0 | 1 | Toggle |
| 1 | 1 | 1 | 0 | Toggle |

### T flip-flop

q(n+1)=t ,. /qn

| **T** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- |
| 0 | 0 | 0 | No change  |
| 0 | 1 | 1 | No change |
| 1 | 0 | 1 | Invert (Toggle) |
| 1 | 1 | 0 | Invert (Toggle) |

## 2. D latch
### VHDL code listing of the process `p_d_latch`
### Listing of VHDL reset and stimulus processes from the testbench `tb_d_latch` file
### Screenshot with simulated time waveforms; always display all inputs and outputs. The full functionality of the entity must be verified.

## 3. Flip-flops
### `p_d_ff_arst`
**VHDL code listing of the process**

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

### `p_d_ff_rst`
**VHDL code listing of the process**

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

### `p_jk_ff_rst`
**VHDL code listing of the process**

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

### `p_t_ff_rst`
**VHDL code listing of the process**

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

## 4 .Shift register

### Image of the shift register schematic. The image can be drawn on a computer or by hand. Name all inputs, outputs, components and internal signals


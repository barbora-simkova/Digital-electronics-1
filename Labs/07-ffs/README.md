## 1. Preparation tasks

![equations](Images/equations.png)

### D flip-flop
| clk | **D** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- | :-: |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | Reset |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | No change |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | Set |

### JK flip-flop

| clk | **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-: | :-- | :-: |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | 0 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 1 | 1 | No change |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 0 | 0 | Reset |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | 0 | Reset |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 0 | 1 | Set |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | 1 | Set |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | 1 | Toggle |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 1 | 0 | Toggle |

### T flip-flop

| clk | **T** | **Qn** | **Q(n+1)** | **Comments** |
| :-: | :-: | :-: | :-- | :-: |
| ![rising](Images/eq_uparrow.png) | 0 | 0 | 0 | No change  |
| ![rising](Images/eq_uparrow.png) | 0 | 1 | 1 | No change |
| ![rising](Images/eq_uparrow.png) | 1 | 0 | 1 | Invert (Toggle) |
| ![rising](Images/eq_uparrow.png) | 1 | 1 | 0 | Invert (Toggle) |

## 2. D latch
### VHDL code listing of the process `p_d_latch`

```vhdl
 p_d_latch : process (d, arst, en)
    begin
        if (arst = '1') then
            q     <= '0';
            q_bar <= '1';
        elsif (en = '1') then
            q     <= d; 
            q_bar <= not d;
        end if;
    end process p_d_latch;
```

### Listing of VHDL reset and stimulus processes from the testbench `tb_d_latch` file

```vhdl
 p_reset_gen : process
    begin
        s_arst <= '0';
        wait for 53 ns;
        
        s_arst <= '1';
        wait for 5 ns;

        s_arst <= '0';
        wait for 108 ns;
            
        s_arst <= '1';
        
        wait;
    end process p_reset_gen;
    
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        s_en <= '0';
        s_d  <= '0';
        
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        s_en <= '1';
        
        wait for 3 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 73ns" severity error;
        
        --d sequence
        wait for 7 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_en <= '0';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        wait for 10 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 160ns" severity error;
        
        --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        s_en <= '1';
        
        wait for 10 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 240ns" severity error;
        
        --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot with simulated time waveforms; always display all inputs and outputs. The full functionality of the entity must be verified.

![d_latch](Images/d_latch.png)

## 3. Flip-flops
### `p_d_ff_arst`
**VHDL code listing of the process**

```vhdl
 p_d_ff_arst : process (clk, arst)
    begin
        if (arst = '1') then
            q     <= '0';
            q_bar <= '1';
        elsif rising_edge(clk) then
            q     <= d; 
            q_bar <= not d;
        end if;
    end process p_d_ff_arst;
```

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

```vhdl
  --------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    --- WRITE YOUR CODE HERE
    p_reset_gen : process
    begin
        s_arst <= '0';
        wait for 58 ns;
        
        s_arst <= '1';
        wait for 15 ns;

        s_arst <= '0';
        wait for 58 ns;
        
        s_arst <= '1';

        wait;
    end process p_reset_gen;


    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        wait for 5 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 75ns" severity error;
        
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        wait for 5 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 150ns" severity error;
               
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
               
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```



**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

![pdffarst](Images/p_d_ff_arst.png)  

### `p_d_ff_rst`
**VHDL code listing of the process**

```vhdl
 p_d_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                q     <= '0';
                q_bar <= '1';
            else
                q     <= d;
                q_bar <= not d;
            end if;
        end if;     
    end process p_d_ff_rst;
```

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

```vhdl
	--------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    --- WRITE YOUR CODE HERE
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 58 ns;
        
        -- Reset activated
        s_rst <= '1';
        wait for 15 ns;

        -- Reset deactivated
        s_rst <= '0';
        wait for 58 ns;
        
        s_rst <= '1';

        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        
        wait for 3 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 63ns" severity error;
        
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        wait for 3 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 76ns" severity error;
        
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
        wait for 4 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 150ns" severity error;
               
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
        
         --d sequence
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        s_d  <= '1';
        wait for 10 ns;
        s_d  <= '0';
        wait for 10 ns;
        --/d sequence
               
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The f ull functionality of the entities must be verified**

![pjkffrst](Images/p_d_ff_rst.png)

### `p_jk_ff_rst`

**VHDL code listing of the process `p_jk_ff_rst`**

```vhdl
p_jk_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q <= '0';
            else
                if (j = '0' and k = '0') then
                    s_q <= s_q;
                    
                elsif (j = '0' and k = '1') then
                    s_q <= '0';
                    
                elsif (j = '1' and k = '0') then
                    s_q <= '1';
                    
                elsif (j = '1' and k = '1') then
                    s_q <= not s_q;
                    
                end if;
                
            end if;
        end if;
    end process p_jk_ff_rst;

    q     <= s_q;
    q_bar <= not s_q;
```

**Listing of VHDL clock, reset and stimulus processes from the testbench files `p_jk_ff_rst`**

```vhdl
	--------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 58 ns;
        
        s_rst <= '1';
        wait for 15 ns;

        s_rst <= '0';
        
        wait;
    end process p_reset_gen;
    
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        wait for 13 ns;
        s_j  <= '0';
        s_k  <= '0';
        wait for 20 ns;
        s_j  <= '0';
        s_k  <= '1';
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '0';
        
        wait for 7 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 60ns" severity error;
        
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '1';
        
        wait for 13 ns;
        s_j  <= '0';
        s_k  <= '0';
        
        wait for 7 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 100ns" severity error;
        
        wait for 20 ns;
        s_j  <= '0';
        s_k  <= '1';
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '0';
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '1';
        
        wait for 13 ns;
        s_j  <= '0';
        s_k  <= '0';
        wait for 20 ns;
        s_j  <= '0';
        s_k  <= '1';
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '0';
        wait for 20 ns;
        s_j  <= '1';
        s_k  <= '1';
        
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

![pjkffrst](Images/p_jk_ff_rst.png)

### `p_t_ff_rst`
**VHDL code listing of the process**

```vhdl
p_t_ff_rst : process (clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q <= '0';
            elsif (t = '1') then
                s_q <= not s_q;
            elsif (t = '0') then
                s_q <= s_q;
            end if;
        end if;   
    end process p_t_ff_rst;
    
    q       <= s_q;
    q_bar   <= not s_q;
```

**Listing of VHDL clock, reset and stimulus processes from the testbench files**

```vhdl
--------------------------------------------------------------------
    -- Clock generation process
    --------------------------------------------------------------------
    p_clk_gen : process
    begin
        while now < 750 ns loop         -- 75 periods of 100MHz clock
            s_clk_100MHz <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk_100MHz <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;
    
    --------------------------------------------------------------------
    -- Reset generation process
    --------------------------------------------------------------------
    --- WRITE YOUR CODE HERE
    p_reset_gen : process
    begin
        s_rst <= '0';
        wait for 65 ns;
        
        -- Reset activated
        s_rst <= '1';
        wait for 30 ns;

        -- Reset deactivated
        s_rst <= '0';
        wait for 20 ns;
        
        s_rst <= '1';
        wait for 30 ns;
        
        s_rst <= '0';
        
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        
        --t sequence
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        --/t sequence
        
        wait for 5 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 75ns" severity error;
        
         --t sequence
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        
        wait for 5 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 110ns" severity error;
        
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        
        wait for 5 ns;
        assert(s_q = '0' and s_q_bar = '1')
        report "Test failed for input on 135ns" severity error;
        
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        --/t sequence
               
         --t sequence
        wait for 10 ns;
        s_t  <= '1';
        
        wait for 5 ns;
        assert(s_q = '1' and s_q_bar = '0')
        report "Test failed for input on 170ns" severity error;
        
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        --/t sequence
        
        --t sequence
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        s_t  <= '1';
        wait for 10 ns;
        s_t  <= '0';
        wait for 10 ns;
        --/t sequence
               
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;

```

**Screenshot, with simulated time waveforms; always display all inputs and outputs. The full functionality of the entities must be verified**

![pjkffrst](Images/p_t_ff_rst.png)

## 4 .Shift register

### Image of the shift register schematic. The image can be drawn on a computer or by hand. Name all inputs, outputs, components and internal signals


# VHDL-BoothMultiplier

    library IEEE;
    use IEEE.STD_LOGIC_1164.ALL;
    use ieee.numeric_std.all;
    --------------------------------------------------------------
    entity booth is	
  	GENERIC ( count : integer := 4);
	  port(Q, X: in std_logic_vector(3 downto 0);
		  QX: out std_logic_vector(7 downto 0));
    end booth;
      --------------------------------------------------------------
    architecture Behavioral of booth is
    begin
  	process(Q, X)
	  variable QXW: std_logic_vector(8 downto 0);
	  variable i:integer;
	   begin
		  QXW:= "000000000" ;
		  QXW(4 downto 1) := Q;
		  for i in 1 to count loop
			 if((QXW(1) = '1' and QXW(0) = '1')or ((QXW(1) = '0' and QXW(0) = '0'))) then
				null;
				
			 elsif(QXW(1) = '1' and QXW(0) = '0') then
				  QXW(8 downto 5) := std_logic_vector(unsigned(QXW(8 downto 5)) - unsigned(X));
				  
			 elsif(QXW(1) = '0' and QXW(0) = '1') then
				  QXW(8 downto 5) := std_logic_vector(unsigned(QXW(8 downto 5)) + unsigned(X));
				  
			 end if;
			 
			 QXW(7 downto 0) := QXW (8 downto 1);
			 
		  end loop;
		  
		  QX(7 downto 0) <= QXW(8 downto 1);
		  
	  end process; 
    end Behavioral;


#TestBench


    LIBRARY ieee;
    USE ieee.std_logic_1164.ALL;
 
    -- Uncomment the following library declaration if using
    -- arithmetic functions with Signed or Unsigned values
    --USE ieee.numeric_std.ALL;
 
    ENTITY tb IS
    END tb;
 
    ARCHITECTURE behavior OF tb IS 
 
    -- Component Declaration for the Unit Under Test (UUT)
 
    COMPONENT booth
    PORT(
         Q : IN  std_logic_vector(3 downto 0);
         X : IN  std_logic_vector(3 downto 0);
         QX : OUT  std_logic_vector(7 downto 0)
        );
    END COMPONENT;
    

    --Inputs
     signal Q : std_logic_vector(3 downto 0) := (others => '0');
    signal X : std_logic_vector(3 downto 0) := (others => '0');

 	--Outputs
    signal QX : std_logic_vector(7 downto 0);
     -- No clocks detected in port list. Replace <clock> below with 
     -- appropriate port name 
 
    --  constant <clock>_period : time := 10 ns;
 
    BEGIN
 
	-- Instantiate the Unit Under Test (UUT)
     uut: booth PORT MAP (
          Q => Q,
          X => X,
          QX => QX
        );

     -- Clock process definitions
    --   <clock>_process :process
    --   begin
    --		<clock> <= '0';
    --		wait for <clock>_period/2;
    --		<clock> <= '1';
    --		wait for <clock>_period/2;
    --   end process;
 

     -- Stimulus process
     stim_proc: process
    begin		
      -- hold reset state for 100 ns.
       -- wait for <clock>_period*10;
		Q <= "0111";-- 3
		X <= "0011";-- 7
		wait for 10 ns;
		Q <= "1100";-- -4
		X <= "1110";-- -2
		wait for 10 ns;
		Q <= "1010";-- -6
		X <= "0101";-- 5
      -- insert stimulus here 

      wait;
     end process;

    END;


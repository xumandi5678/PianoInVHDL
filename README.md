# PianoInVHDL
This is the code for programming a Digilent FGPA Basys3 to play certain "notes" at different frequencies 

### Mandi Xu 
### ELEN E3082
### Digital Systems Laboratory 

# Lab 6 Report 

1) Video of the Design: 
https://youtu.be/_C6RzRe2jPk 
When I recorded this video, I realized I recorded it vertically with the camera pointed at the speaker and the display but I only switched the first switch to begin the autoplay. (This is verifiable as shown in the code).  

2) Name of the Song: 
Wu Ji by Hai Lin (Soundtrack to the show the Untamed) 

3) List of the Notes: 
Music Score: (where I got the notes from the melody on the top bars) 
https://musescore.com/jack_lam/scores/5692852

E4 
C4 
A3
B3
G3 
E3
C3
B3
C4
D4
G3
E3



4) Code: 

Two portions I changed were the signal components and the User Interface in the piano.vhd file 

Figure 1: 

Figure 1. This is a screenshot of the signals. The bottom one on line 111 was the one I added to keep track of the timing for each note. Encoded with 32 bits 0 to 31. 

Figure 2: 
    -- User Interface
    note_in <= note_next; -- the specific note played 
    process (CLK,RST) begin
        if (RST = '1') then
            note_next <= (others => '0');
        elsif (CLK'event and CLK = '1') then 
        
            if(switch = x"00000001") then --the 1st switch on FPGA  
                count <= count + x"00000001"; –- current clock count
            end if;
            -- counting by hexadecimal
            -- below is the note played for specific clock counts 00000000

            if (count > x"00000000" and count < x"5F5E100") then 
                note_next <= "10101"; -- E4
            elsif(count > x"5F5E100" and count < x"BEBC200") then
                note_next <= "10001"; -- C4
            elsif(count > x"BEBC200" and count < x"11E1A300") then 
                note_next <= "01010"; -- A3
            elsif(count > x"11E1A300" and count < x"17D78400") then 
                note_next <= "01100"; -- B3
            elsif(count > x"17D78400" and count < x"1DCD6500") then 
                note_next <= "01000"; -- G3
            elsif(count > x"1DCD6500" and count < x"23C34600") then 
                note_next <= "00101"; -- E3 
            elsif(count > x"23C34600" and count < x"29B92700") then 
                note_next <= "00001"; -- C3 
            elsif(count > x"29B92700" and count < x"2FAF0800") then 
                note_next <= "01100"; -- B3
            elsif(count > x"2FAF0800" and count < x"35A4E900") then 
                note_next <= "10001"; -- C4 
            elsif(count > x"35A4E900" and count < x"3B9ACA00") then 
                note_next <= "10011"; -- D4
            elsif(count > x"3B9ACA00" and count < x"4190AB00") then 
                note_next <= "01000"; -- G3
            elsif(count > x"4190AB00" and count < x"47868C00") then 
                note_next <= "00101"; -- E3 
            end if;
end if;

end process; 

end Behavioral; 

Figure 2. This is the User Interface portion that I edited. 

Code Function: 

	After synthesis and implementation and programming of the device, when the first switch is flipped, x”00000001”, 
	under hexadecimal notation, then the clock count increases. Each note takes one second to play, so I converted a table
	of values going from 0*10^8 clock counts through 11*10^8 clock counts for each note in the melody (all in hexadecimal). 

	Notes are generated or modified after each count changes into the specific count times. They are formatted using 5 bits
	from the note-gen file that maps the specific notes.
	
	Timing of the notes is determined by the count ranges defined by the if and elsif statements. 1 second is one increase
	by adding 10^8. 
	
	Notes are displayed through the note_in and note_next connection from the seven_seg.vhd file. As you read seg_buf’s
	bits from left to right, it translates into the notes. 


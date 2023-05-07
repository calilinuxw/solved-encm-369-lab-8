Download Link: https://assignmentchef.com/product/solved-encm-369-lab-8
<br>



Please see the Lab 1 instructions.

<h1>Notes about timing in digital logic</h1>

(The material from here to the beginning of the Exercise A instructions is mostly a review of material taught in ENEL 353 every year for the past few years. I originally wrote the material for students who had taken a version of ENEL 353 that had no coverage of timing. I’ve included the material in the 2017 Lab 8 instructions because it may serve as a short and convenient review.)

For desktop computers available in 2017, a typical processor clock frequency is approximately 3GHz, that is, 3 × 10<sup>9 </sup>cycles per second. The period for that clock frequency is 1s <em>/ </em>(3 × 10<sup>9</sup>), so, 0.333 nanoseconds, or 333 picoseconds. That’s just an astonishingly short period of time; it’s hard for us humans to comprehend how short it really is.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

However, it is still interesting to ask why that clock period can’t be squeezed from 333 picoseconds down to 200 picoseconds, or 100 picoseconds. There are two main reasons:

<ul>

 <li>For a given synchronous logic circuit, increasing the clock frequency increases power consumption. If you make the clock frequency too high, the circuit will destroy itself with its own waste heat. We won’t look at power consumption in ENCM 369, but please be aware that it is an important and interesting problem.</li>

 <li>Circuit elements such as combinational gates and D flip-flops are <em>not infinitely fast</em>. For all of them there are very short <em>delays </em>between changes to inputs and responses of outputs.</li>

</ul>

This section reviews simple models of delays for combinational logic and D flipflops; these models should help with understanding limits on clock speeds for the processor designs of Chapter 7 of the course textbook.

The models and terminology for delays are taken from Sections 2.9.1 and 3.5.2 of the course textbook.

<h2>Delays in combinational logic</h2>

For even the simplest combinational element, an inverter, there is a <em>range </em>of possible delays. Response of the output to a change in input depends on various factors, such as …

<ul>

 <li><em>temperature</em>—switching in a circuit tends to be slower when the circuit is warm than when it is cold;</li>

 <li><em>asymmetry</em>—because of transistor physics, a CMOS<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> inverter might make a 1-to-0 output transition faster than it can make a 0-to-1 transition;</li>

 <li><em>manufacturing variations</em>—two supposedly identical gates within the same integrated circuit might switch at different speeds due to imperfections in the material and equipment used to make the circuit;</li>

 <li><em>variations in load</em>—if the output drives only one input of another gate, it will switch faster than it would driving many gate inputs. (Details of loads and a related concept called <em>fanout </em>are topics for third year in Electrical Engineering.)</li>

</ul>

<strong>Figure 1: </strong>Contamination delay and propagation delay for an inverter. The waveform for <em>x </em>shows the two possible transitions for <em>x</em>. The waveform for <em>y </em>can be thought of as showing a <em>collection </em>of responses: the fastest possible, the slowest possible, and some in-between.

<em>x </em>

<strong>Figure 2: </strong>Contamination delay and propagation delay for a 32-bit adder. Of course, the 64 input bits are not required to change all at the same time. Due to the carry logic within the adder, changes to the 32 output bits may be quite spread out in time.

To keep things simple, it makes sense to come up with two numbers that describe the shortest and longest delays possible in response to a change of input to a combinational element. We will call these <em>t</em><sub>cd</sub>, <em>contamination delay</em>, and <em>t</em><sub>pd</sub>, <em>propagation delay</em>.

For a component with multiple inputs and multiple outputs, we need to be careful with our definitions:

<ul>

 <li><em>t</em><sub>cd </sub>is the <em>shortest </em>possible delay between a change in <em>any single </em>input bit and a change in <em>any single </em>output bit.</li>

 <li><em>t</em><sub>pd </sub>is the <em>longest </em>possible delay between the time when <em>all </em>input bits are stable and the time when <em>all </em>output bits have reached final values.</li>

</ul>

These definitions are illustrated for a 32-bit adder in Figure 2.

<h2>A timing model for a D flip-flop</h2>

For a D flip-flop, there are two main timing concerns:

<ul>

 <li>How close in time to an active clock edge can the D input change without causing the “Q copies D on an active clock edge” behaviour to become unreliable? This is usually expressed in terms of two parameters: the <em>setup time</em>, <em>t</em><sub>setup</sub>, and the <em>hold time</em>, <em>t</em><sub>hold</sub>.</li>

</ul>

<strong>Figure 3: </strong>Depiction of setup and hold times for a positive-edge-triggered D flip-flop. The example D input breaks the rule that D must be constant within the sampling window defined by the setup and hold times. The given Q signals are sketches of three of the many possible outcomes: (1) Q starts to move from 0 to 1, but falls back to 0; (2) Q gets to 1, possibly more slowly than a circuit designer would like; (3) Q is “stuck” about halfway between the voltages for 0 and 1 for a significant period of time. Case (3) is an example of an undesirable phenomenon called <em>metastability</em>.

<ul>

 <li>How long after an active clock edge will it take for the Q output to change? The parameters for this are <em>t</em><sub>ccq</sub>—<em>contamination delay from clock to Q</em>, and <em>t</em><sub>pcq</sub>—<em>propagation delay from clock to Q</em>.</li>

</ul>

<strong>Setup and hold times: </strong>It’s not possible to design a circuit to reliably capture the value of a D input on an active clock edge if that D input is not constant for at least some tiny time interval nearby to that active clock edge. <em>t</em><sub>setup </sub>is the time interval before an active clock edge for which D must be constant, and <em>t</em><sub>hold </sub>is the time interval after an active clock edge for which D must be constant. Figure 3 shows some of the things that can go wrong if setup and hold time constraints are not satisfied.

<strong>Clock-to-Q contamination delay and propagation delay: </strong>Informally, for a D flip-flop, we say, “Q samples D on each active clock edge, and holds that sample value until the next active edge.” However, Q can’t actually change state instantly, so if Q changes from 0 to 1 or 1 to 0, that change occurs with a delay that is at least <em>t</em><sub>ccq </sub>and at most <em>t</em><sub>pcq</sub>, as shown in Figure 4.

<strong>Figure 4: </strong>Clock-to-Q contamination delay and propagation delay, illustrated for a positive-edge-triggered D flip-flop with Q changing from 0 to 1. It is assumed here that D satisfies the setup and hold time constraints for the flip-flop. The waveform for Q can be thought of as a collection of responses: fastest possible, some slower ones, and the slowest possible.

<em>D </em><em>Q</em>

<h1>Exercise A: Review of setup- and hold-time constraints</h1>

<h2>Read This First</h2>

This exercise reviews important material from ENEL 353 about timing in digital logic circuits. For help with review of that material, you may wish to carefully read Section of this lab assignment document and Sections 2.9.1, 3.5.1, and 3.5.2 in the course textbook.

<h2>What to Do, Part I: Setup-time constraint</h2>

Consider the circuit of Figure 5. Suppose that for the registers <em>t</em><sub>pcq </sub>is 25ps and <em>t</em><sub>setup </sub>is 32ps. Suppose that for the sign-extension unit <em>t</em><sub>pd </sub>is 45ps, and for the multiplier <em>t</em><sub>pd </sub>is 171ps.

If the desired frequency for clk is 3.00GHz, what is the maximum acceptable <em>t</em><sub>pd </sub>for the adder?

<h2>What to Do, Part II: Hold-time constraint</h2>

Again consider the circuit of Figure 5. If <em>t</em><sub>hold </sub>for the registers is 6ps, what is the minimum acceptable <em>t</em><sub>ccq </sub>value for the registers?

<strong>What to hand in</strong>

Hand in clear and precise calculations of you answers to Part I.

<h1>Exercise B: Propagation delay in processor circuits</h1>

This exercise will not be marked, because I expect that solutions to it are not hard to find online. However, it’s <em>important</em>, and it would be reasonable to expect to see a related problem or two on the final exam.

<strong>Figure 5: </strong>Schematic for part of a hypothetical computer design, with an instruction set that is not MIPS.

<strong>Figure 6: </strong>Model for propagation delay in a multiplexer. <em>t</em><sub>pd </sub>is the longest possible delay from a change in the select signal (<em>S </em>in this example) or the <em>selected </em>data input (<em>D</em><sub>1 </sub>in this example), whichever happens later, to the last change in the output signal. <em>Unselected </em>data inputs (<em>D</em><sub>0 </sub>in this example) do not affect the output.

<h2>Read This First</h2>

It’s probably useful to refer to Figure 7.11 in your textbook while you read this section.

For a complex combinational component such as an ALU, <em>t</em><sub>pd </sub>can be defined as the longest possible delay between the last change on an input wire and the moment when all output wires have correct values. (In fact, I’ve already made that definition and illustrated it for the example of an adder in Figure 2 on page 3.)

Note that a multiplexer is a special case in which it is not necessary for all input wires to have stable signals before the output is ready. See Figure 6 for an explanation.

For complex sequential components such as the PC, register file, and data memory in the computer designs of Chapter 7 of the course textbook, there will be setup time parameters.

For example, if <em>t</em><sub>RFsetup </sub>= 20ps is specified for the register file, that means that a write to a GPR is reliable as long as these three things are true for at least 20ps in advance of an active clock edge:

<ul>

 <li>RegWrite = 1;</li>

 <li>the 5-bit WriteReg signal (which selects the destination GPR) is stable;</li>

 <li>the 32-bit Result signal (which provides the bit pattern to be copied into the destination GPR) is stable.</li>

</ul>

To give another example, if <em>t</em><sub>pcq </sub>= 30ps is specified for the PC, that means that the address input to the instruction memory is ready no later than 30ps after an active clock edge.

<h2>What to Do</h2>

Carefully review the timing analysis of Section 7.3.4 in the textbook, which finds a minimum clock period of 925ps for the single-cycle processor.

Note that the following assumptions have been made: (1) reading the register file (<em>t</em><sub>RFread</sub>) takes longer than sign-extend and a mux combined (as stated in the textbook); (2) reading the register file (<em>t</em><sub>RFread</sub>) takes longer than generating Control Unit outputs (assumed but not stated in the book). Please use those assumptions in completing this exercise.

It’s pretty clear that the lw instruction must be the one that puts a lower limit on the clock period, because lw has two memory-read delays, but all other instructions have only one. Nevertheless, it’s good exercise to determine minimum clock periods for other instructions.

<strong>Part I. </strong>Using the information in Table 7.6 on page 389 of the textbook, determine the minimum clock period for correct handling of R-type instructions.

<strong>Part II. </strong>Determine the minimum clock period for correct handling of beq instructions, using the information in textbook Table 7.6, and the following extra information:

<table width="133">

 <tbody>

  <tr>

   <td width="82"><strong>element</strong></td>

   <td width="51"><em>t</em><strong>pd</strong></td>

  </tr>

  <tr>

   <td width="82">adder</td>

   <td width="51">162ps</td>

  </tr>

  <tr>

   <td width="82">sign extend</td>

   <td width="51">35ps</td>

  </tr>

  <tr>

   <td width="82">shift left</td>

   <td width="51">20ps</td>

  </tr>

  <tr>

   <td width="82">and gate</td>

   <td width="51">18ps</td>

  </tr>

 </tbody>

</table>

Note that there are two paths to check: first, a path involving the register file, the ALU, and some other elements; second, a path involving two adders and some other elements.

<h2>What to Hand In</h2>

Nothing. You can check your work against a solution that will be posted by Tuesday morning, March 13.

<h2>Remarks</h2>

The results of Parts I and II will confirm that lw really does determine the overall minimum safe clock period. (We haven’t checked sw, but sw will clearly work if lw works.)

The clock period is <em>constant</em>—it is <em>not practical </em>for the clock to somehow change its period from one cycle to the next, depending on what kind of instruction is being executed.

<h1>Exercise C: Tracing instructions though a pipelined processor</h1>

<h2>Read This First</h2>

To learn how a pipelined processor works, it is helpful to trace bit patterns as they move through the pipeline registers.

<h2>What to Do</h2>

Suppose the processor of textbook Figure 7.47 is implemented with a 2GHz clock, so its clock period is 0<em>.</em>5ns. Suppose further that at <em>t </em>= 36<em>.</em>0ns after a program starts running, a positive clock edge occurs that starts the Fetch phase of the lw instruction in the following program fragment:

<table width="347">

 <tbody>

  <tr>

   <td width="135"><em>instruction address</em></td>

   <td width="212"><em>instruction            disassembly</em></td>

  </tr>

  <tr>

   <td width="135">0x0040_0120</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

  <tr>

   <td width="135">0x0040_0124</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

  <tr>

   <td width="135">0x0040_0128</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

  <tr>

   <td width="135">0x0040_012c</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

  <tr>

   <td width="135">0x0040_0130</td>

   <td width="212">0x8d09_0014 lw $9, 20($8)</td>

  </tr>

  <tr>

   <td width="135">0x0040_0134</td>

   <td width="212">0x01ab_5025 or $10, $13, $11</td>

  </tr>

  <tr>

   <td width="135">0x0040_0138</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

  <tr>

   <td width="135">0x0040_013c</td>

   <td width="212">0x0000_0000 nop</td>

  </tr>

 </tbody>

</table>

The term nop is short for “no operation”. The machine code for nop indicates an R-type instruction that will attempt to write to $0, which is guaranteed to have no effect.

The above sequence of instructions is unlikely to appear in a real program, but all the nop instructions make this exercise less messy than it would be without the nops.

Assume the following is true at <em>t </em>= 36<em>.</em>0ns:

$8 = 0x1001_0300, $9 = 0x0000_0123, $10 = 0x2345_6789, $11 = 0x2233_44c0, $13 = 0x0000_0025.

The word at data memory address 0x1001_0314 is 6677_8899.

Answer questions 2–5 below. Question 1 is answered as a model for how to answer all the other questions. Be sure to give valid <em>reasons </em>for your answers— please don’t just write out a bunch of numbers. Use hexadecimal notation for 32-bit numbers and base two for 5-bit numbers.

<ol>

 <li>What gets written into the PC at <em>t </em>= 36<em>.</em>5ns? And what will be the value of InstrD very shortly after <em>t </em>= 36<em>.</em>5ns?</li>

</ol>

<em>Answer: That is the end of the Fetch stage of the </em><em>lw instruction, and the beginning of the Decode stage. The PC gets the output of the adder in the Fetch stage, which will be </em><em>0x0040_0134. InstrD gets the instruction, which is </em><em>0x8d09_0014.</em>

<ol start="2">

 <li>What gets written into the PC at <em>t </em>= 37<em>.</em>0ns? And what will be the value of InstrD very shortly after <em>t </em>= 37<em>.</em>0ns?</li>

</ol>

Shortly before <em>t </em>= 37<em>.</em>0ns, what are the values of the following three signals, which are about to be written into the D/E pipeline register: RD1, RD2, and the output of Sign Extend?

<ol start="3">

 <li>Shortly before <em>t </em>= 37<em>.</em>5ns, what are the values of the following four signals, which are about to be written into the D/E pipeline register: RD1, RD2, InstrD20:16, and InstrD15:11?</li>

</ol>

Also shortly before <em>t </em>= 37<em>.</em>5ns, what is the value of the the 32-bit ALU output?

<ol start="4">

 <li>Shortly before <em>t </em>= 38<em>.</em>0ns, what are the values of the following signals: the 32-bit ALU output and WriteRegE<sub>4:0</sub>?</li>

 <li>Shortly before <em>t </em>= 38<em>.</em>5ns, what are the values of the following signals:</li>

</ol>

ALUOutM and WriteRegM<sub>4:0</sub>?

<h1>Exercise D: Forwarding</h1>

<h2>What to Do</h2>

Using a drawing similar to Figure 7.49 on page 416 of the textbook, show the forwarding paths needed to execute the following sequence of five instructions:

and                 $t0, $t1, $t2 add   $s0, $s1, $t0 sw    $t0, 12($sp) lw      $a0, 0($s0) sub      $t9, $s3, $s4 add  $a1, $a0, $t9

You can make the style of your drawing much simpler than what is in the textbook! There is no need to draw fancy little ALUs and pipeline registers—just draw boxes for pipeline stages and then show all the forwarding.

<a href="#_ftnref1" name="_ftn1">[1]</a> An Olympic sprinter is considered to have false-started if he or she reacts to the starting gun in less than 0.1 seconds; for a 3GHz clock, 300,000,000 clock cycles happen in that 0.1-second interval.

<a href="#_ftnref2" name="_ftn2">[2]</a> CMOS stand for Complementary MOS, and MOS stands for metal-oxide-semiconductor, a kind of transistor. (Confusingly, modern MOS transistors do not contain any metal!) CMOS logic has been the dominant technology for digital integrated circuits for over three decades.
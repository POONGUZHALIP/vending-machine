# vending-machine
// Code your testbench here
// or browse Examples
// Code your testbench here
// or browse Examples
module vending(clk,reset,money,balance,sel,cal,PA,PB,PC);
  input clk,reset;
  input [1:0] money;
  input cal;
  input [3:0] sel;
  output reg [1:0]balance;
  output reg[1:0] PA;
  output reg PB,PC;
  parameter A=2'b00;
  parameter B=2'b01;
  parameter C=2'b10;
  parameter D=2'b11;
  reg [1:0] p_state,n_state;
  always@ (posedge clk)
    begin
      if(reset == 1)
        begin
          balance = 2'b00;
          PA=0;
          PB=0;
          PC=0;
          p_state=A;
          n_state=A;
          $display("WELCOME");
          $display("ONLY SUPPORT 5 RS,10 RS ,15 RS");
        end
    else 
      begin
        p_state=n_state;
        case(p_state)
          A: begin
            if(money==2'b01)
            begin
              n_state=B;
            end
            if(money==2'b10)
            begin
              n_state=C;
            end
            if(money==2'b11)
            begin
              n_state=D;
            end
          end
         B: begin
           if(sel==4'b0001)
             begin
               PA=2'b01;
               balance=2'b00;
               n_state=A;
               $display("you have chose A product");
             end
           if(cal==1)
             begin
               PA=0;
               PB=0;
               PC=0;
               balance=2'b01;
               n_state=A;
               $display("youR ORDER IS CANCELLED ");
             end
         end 
          C:begin
            if(sel==4'b0010)
              begin
                PB=1;
                balance=2'b00;
                n_state=A;
              end
            if(sel == 4'b011)
              begin
                PA=1;
                balance =2'b01;
                n_state=A;
              end
            if(sel==4'b0100)
              begin
                PA=2;
                balance =2'b00;
                n_state=A;
              end
            if(cal==1)
              begin
                PA=0;
                PB=0;
                PC=0;
                n_state=A;
                balance=2'b10;
              end
                
          end
          D:begin
            if(sel==4'b0101)
              begin
                PC=1;
                balance=2'b00;
                n_state=A;
              end
            if (sel==4'b0110)
              begin
                PA=1;
                PB=1;
                balance=2'b00;
                n_state=A;
              end 
            if(sel==4'b0111)
              begin
                PA=2;
                balance=2'b01;
                n_state=A;
              end
            if(sel==4'b1000)
              begin
                PB=1;
                balance=2'b01;
                n_state=A;
              end
            if(cal==1)
              begin
                PA=0;
                PB=0;
                PC=0;
                balance=2'b11;
                n_state=A;
              end
          end
        endcase
      end                       
   end
 endmodule
          
module top;
  reg clk,reset;
  reg [1:0] money;
  reg cal;
  reg [3:0] sel;
  wire [1:0] balance;
  wire [1:0] PA;
  wire PB,PC;
  vending v1(clk,reset,money,balance,sel,cal,PA,PB,PC);
  initial begin
    clk=0;
    forever #5 clk=~clk;
  end
  initial begin
    reset=1;
    #15;
    reset=0;
  end
  initial begin
    #20;
    money=2'b01;
   sel=4'b0001;
cal=1;
    #30;
    $finish;
  end 
  initial begin
    #40
    $display("money=%b sel=%b cal=%b PA=%b PB=%b PC=%b balance=%b",money,sel,cal,PA,PB,PC,balance);
    $display("THANK YOU");
  end
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end 
endmodule

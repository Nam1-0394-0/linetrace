
#include <xc.h> 

#include <pic18f25k50.h>

   //----------------------------------------------------------------------------------------------

 int b_rotate = 5;//when it lost line, set motor's current 4:16 (right or left)
 int forward = 15;//typical speed

 int b_rotate_2 = 9;
 int forward_2 = 10;
 
 int b_rotate_3 = 6; //revival
 int forward_3 = 10; //revival
 
 int b_forward = 10;
 
 int last_action = 0; // 0: straight, 1: left, 2: right
 int sensor_sum = 5; // blackline:sensor_sum=5,4,3 whiteline:sensor_sum=2,1,0

 int stop() {
    LATC = 0x00; // モーター停止
}
 
  int straight(int p){

        if(p<b_forward)//accelate in straight line

                     LATC=0x03;

                 else

                     LATC=0x00;

                      }

  int b_right(int p){

                  if(p<b_rotate){

                     LATC=0x01;//right

                 }else{

                     LATC=0x00; //right

                 }                 if(p<forward){//typical speed

                     LATC=0x02; ////left

                 }else{

                     LATC=0x00; ////left

                 }     }
        
  int b_right_2(int p){

                  if(p<b_rotate_2){

                     LATC=0x01;//right

                 }else{

                     LATC=0x00; //right

                 }                 if(p<forward_2){//typical speed

                     LATC=0x02; ////left

                 }else{

                     LATC=0x00; ////left

                 }     }
  
  //revival
  int b_right_3(int p){

                  if(p<b_rotate_3){

                     LATC=0x01;//right

                 }else{

                     LATC=0x00; //right

                 }                 if(p<forward_3){//typical speed

                     LATC=0x02; ////left

                 }else{

                     LATC=0x00; ////left

                 }     }

  int b_left(int p){

                  if(p<b_rotate){

                     LATC=0x02;////left

                 }else{

                     LATC=0x00; ////left

                 }

                 if(p<forward){//typical speed

                     LATC=0x01; //right

                 }else{

                     LATC=0x00; //right

                 }     }

  int b_left_2(int p){

                  if(p<b_rotate_2){

                     LATC=0x02;////left

                 }else{

                     LATC=0x00; ////left

                 }

                 if(p<forward_2){//typical speed

                     LATC=0x01; //right

                 }else{

                     LATC=0x00; //right

                 }     }

  //revival
  int b_left_3(int p){

                  if(p<b_rotate_3){

                     LATC=0x02;////left

                 }else{

                     LATC=0x00; ////left

                 }

                 if(p<forward_3){//typical speed

                     LATC=0x01; //right

                 }else{

                     LATC=0x00; //right

                 }     }
     //-----------------------------------------------------------------------------------------------------------------

 main(void) {

          int p=0; //parameter for making pulse wave

    //setting digital ports

    ADCON1 = 0x0F;//16

    //setting input or output

    TRISA = 0x00;            //  portA   all output

    TRISB = 0xFF;            //  portB   all input

    TRISC = 0x00;            //  portC   all output

    //reset to 0

    PORTA = 0x00;            //  portA reset to 0

    PORTB = 0x00;            //  portB reset to 0

    PORTC = 0x00;            //  portC reset to 0

   //----------------------------------------------------------------------------------------------------------------generate pseudo pulse

    while(1){

        p++;//count up

         if(p>=30){//loop counting

            p=0;

        }

        PORTA &= 0xe0; // reset ports

        PORTA |= (PORTB & 0x1f); // PORTB(RB0toRB4) copy to PORTA(RA0 to RA4)

               
        
//black line trace
//left turn        
            if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 == 0){//the black line is at center and right1
              
                b_left(p);
                last_action = 1;
                sensor_sum=4;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0){//the black line is at center and right1
            
                b_left_2(p);
                last_action = 1;
                sensor_sum=3;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 != 0){//the black line is at center and right1
            
                b_left_2(p);
                last_action = 1;
                sensor_sum=3;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 != 0){//the black line is at center and right1
            
                b_left_2(p);
                last_action = 1;
                sensor_sum=3;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0 && last_action == 1){//the black line is at center and right1
          
                b_left_2(p);
                last_action = 1;
                sensor_sum=2;
            
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 != 0 && last_action == 1){//the black line is at center and right1
          
                b_left_2(p);
                last_action = 1;
                sensor_sum=2;    

//right turn                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0){//the black line is at center and left1
             
                b_right(p);
                last_action = 2;
                sensor_sum=4;
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0){//the black line is at center and left1
           
                b_right_2(p);
                last_action = 2;
                sensor_sum=3;
            
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0){//the black line is at center and left1
           
                b_right_2(p);
                last_action = 2;
                sensor_sum=3;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0){//the black line is at center and left1
           
                b_right_2(p);
                last_action = 2;
                sensor_sum=3;    
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0 && last_action == 2){//the black line is at center and left1
             
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 != 0 && last_action == 2){//the black line is at center and left1
             
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;
                
//revival
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0 && last_action==2){

                b_right_3(p);
                last_action = 2;
                sensor_sum=0;
              
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0 && last_action==1){

                b_left_3(p);
                last_action = 1;
                sensor_sum=0;  

                

//white line trace                
//left turn
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 != 0){//the black line is at center and right1
             
                b_left(p);
                last_action = 1;
                sensor_sum=1;
             
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0){//the black line is at center and right1
             
                b_left_2(p);
                last_action = 1;
                sensor_sum=2;    
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 == 0){//the black line is at center and right1
             
                b_left_2(p);
                last_action = 1;
                sensor_sum=2;
            
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 == 0){//the black line is at center and right1
             
                b_left_2(p); 
                last_action = 1;
                sensor_sum=2;
                
           
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 != 0 && last_action==1){//the black line is at center and right1
             
                b_left_2(p); 
                last_action = 1;
                sensor_sum=2;
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 == 0 && last_action==1){//the black line is at center and right1
             
                b_left_2(p); 
                last_action = 1;
                sensor_sum=3;    
                
//right turn
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0){//the black line is at center and left1
             
                b_right(p);
                last_action = 2;
                sensor_sum=1;
                
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0){//the black line is at center and left1
              
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;    
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0){//the black line is at center and left1
              
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;
            
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0){//the black line is at center and left1
              
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;
             
            }else if(PORTBbits.RB0 != 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0 && last_action==2){//the black line is at center and left1
              
                b_right_2(p);
                last_action = 2;
                sensor_sum=2;
                
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 != 0 && PORTBbits.RB2 != 0 && PORTBbits.RB3 != 0 && PORTBbits.RB4 == 0 && last_action==2){//the black line is at center and right1
             
                b_right_2(p); 
                last_action = 2;
                sensor_sum=3;        
             
//revival
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0 && last_action==2){

                b_right_3(p);
                last_action = 2;
                sensor_sum=0;
              
            }else if(PORTBbits.RB0 == 0 && PORTBbits.RB1 == 0 && PORTBbits.RB2 == 0 && PORTBbits.RB3 == 0 && PORTBbits.RB4 == 0 && last_action==1){

                b_left_3(p);
                last_action = 1;
                sensor_sum=0;
              
            }else{
                
                straight(p);
                
              
            } 
    }
 }

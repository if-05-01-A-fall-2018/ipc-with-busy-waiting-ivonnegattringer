# Race Conditions
All in all a race condition develops if two ore more processes want to write something on your disk at the same time and they got the same location where to save something. this is also called entering a critical region. So if both write on the same location on your disk, one data of a thread will be lost.

# Disabling Interrupts
  1. Though none of the process can be interrupted, on multi-core machines, more than one process can be run at the same time. Thus there can also two processes enter the critical region at the same time.
  2. Why? If a process is caught in a while, needing some other information from another process, which doesn't get it's time to work, the process will wait endless on a single core machine.

# Peterson's solution
  1. Scenarios
   ..1. -Process 0 calls enter_region():
      other = 1;
      interested[0] = true;
      loser = 0;
      while(loser == process && interested[other]);
    the other is not interested so far, so process0 leaves the function and is now in the critical region.
    -Process 1 calls enter_region():
      other = 0;
      interested[1] = true;
      loser = 1;
      while(loser == process && interested[other]);
    since process 0 is interested, process 1 stays in the while loop.
  ..2. -Process 0 calls leave_region():
      interested[0] = false;
    process 1 leaves the waiting loop now and enters the critical region.
  it is nearly the same as the first one, but this time you cannot say which of the processes will enter the critical region first. the one who writes on the loser variable the latest, will be the loser. the other will be able to enter the region.
  2. since loser just can have one value - also when two processes at the same time write something on the same location, our nice scheduler will decide for one. :) The only problem that I see is, if the process in the critical region fails, or whatever and does not go to the point where it could say, Ok i am not interested anymore. So when another process is waiting, they'll wait endless.
  3. So, the loser variable determines which of the processes have to wait when both are interested. Loser just has a effect, when both are interested as said before.
  #### 4.
  int loser ;
  Bool interested[3];
  int secondloser;
  void enter_region(int process) {
    int otherProcess = 2 - process;
    if(process == 1){
       otherProcess = 2;
     }
     int otherSecoundProcess = 1 - process;
     if (process == 2) {
       otherSecoundProcess = 1;
     }
     loser = process;
     while((loser == process && interested[otherProcess] && interested[otherSecoundProcess]) );
     secondloser = process;
     while(!(loser == secondloser) && process == secondloser && interested[3-(secondloser+loser)]);
  }

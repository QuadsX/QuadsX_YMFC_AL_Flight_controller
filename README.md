# QuadsX_YMFC_AL_Flight_controller
A performance revision to Joop Brokking's Open-Source YMFC-AL-Flight-controller - Updated ISR

  09/17/2016 - QuadsX.com - this is a re-release of Joop Brokking's YMFC-AL Open-Source QuadCopter Flight Controller and is made available on github 
  
  Version 1.0.1 - QuadsX_YMFC_AL_Flight_Controller.ino
  
  
  I have been in direct communication with Joop about this release and feature upgrades and thanked him for his great contribution to the RC hobby.
  
  I can say that Joop's code-base is truly a great place to start with remote-controlled multi-rotor open-source fun flying!
  
  The math is solid, the code is well formed and provided a nice place to add features which will appear in upcoming QuadsX releases.
  
  This code is pin-compatible with YMFC-AL and YMFC-3D hardware
  
  NOTE:
  The next release of this code will add control of a REAL-TIME TILT SERVO for an FPV CAMERA!!! - been testing this for a few weeks and its time to share
  (FUTURE RELEASES will not be pin-compatible with the original YMFC-AL and YMFC-3D hardware, but adds desireable features)
  
  ~
  
  THINGS TO THINK ABOUT AND OTHER OPEN SOURCE PROJECTS
  
  As with any flight controller your RC Transmitter must be sending the right timing pulses to create the proper control environment.
  Most RC Transmitters are not calibrated well when they are received / out of the box.
  Many RC Transmitters do not display the actual timing values when making travel/end-point adjustments.
  Joops's YMFC-AL (and YMFC-3D) code inspired me to create a standalone sketch for the arduino to allow the PPM output of your RC Transmitter to help calibrate the
  timing to absolute accuracy.
  What you get is the best starting point to run this or any flight controller by truly calibrating your RC Transmitter configuration before you start anything else.
  
  IMPORTANT NOTE:
  Also, this is important when using Joop's code or THIS code as the values which determine low, mid, high, for gimbal channels are stored in EEPROM during the
  YMFC-AL Calibration and Setup Sketch - this means that the values stored in during the INITIAL SETUP really need to be correct - additionally, if values are not correct,
  - or - your RC Transmitter is changed or re-calibrated after initial setup, it may have effects/conflicts on that which is stored in EEPROM.
  
  
  >>>> Be sure to download and use the 'QuadsX_PPM_Pulse_Timing_Calibrator.ino' sketch to help calibrate your RC Transmitter to precise timing specifications. <<<<<<
  
  -----------------------------------------------------------------------------------------------------------------------
  
  Version 1.0.1 - QuadsX_YMFC_AL_Flight_Controller.ino
  
  This code is pin-compatible with YMFC-AL and YMFC-3D hardware
  
  RELEASE NOTES:
  
  This code uses #defines - see below within the code - to set several options on how the code behaves/integrates features as noted below.
  
  
  This code offers upgrades to the ISR for more efficient handling of events
  
    Choice of original ISR handler with speed upgrade
   Or
    Replacement ISR handler with more speed upgrade
     - this version allows significant reduction in ISR time and allowed the development of the real-time camera servo code for the next release!
     ??? what is different ???
     Joop's code functioned well, but did not execute efficiently due to it's methodology
     in each pass through the ISR the original code would make decisions on what to do, but not stop and return at the point of completion
     instead, each pass throughthe ISR made numerous tests (if statements) which were not going to be valid if a prior test was already valid...
     the code uses 'return' statements to create a good order of efficiency increase, sufficient to get useful clock time for other features
     
  Added #defines for HANDLING ISSUES WITH STARTUP - YAW - THROTTLE VALUES and cheaper/un-calibrated tx's
  i.e. see -> RECEIVER_PPM_* in code below
     
 
  Changes and modifications from original YMFC-AL code have been thoroughly tested, timing has been extensively analyzed with oscilloscopes and logic analyzers.
  
  Custom built 250, 450, and 550 scale QuadCopters have been flying based on these code changes and upgrades.
  
  This code base has been built and tested on several releases of the Arduino IDE - most recently 1.6.0
  
  
  USE NOTES:
  in flight, the original YMFC-AL code was completely capable of PID tuning and obtaining great flight results.
  
  in use, after tuning and flight, back on the bench it seemed as though the best tuning was also taking the most time - i.e. using up the clock to the point
  of the ESC loop becoming just slighty jerky on the scope, which said to me that as the PID values change, the resultant (8bit) math took more time (values changing
  making the AVR run more cycles to compute and return.
  
  i sought to resolve this efficiency for the purpose of adding additional features to the YMFC-AL (or YMFC-3D) code base.
  
  my first attemtp to add a pin driving a servo to drive an fpv camera's tilt made it clear that there was some timing problems after adding the servo.
  
  it became clear after enhancing the ISR routine that there was some PID tuning required - that was accomplished and the servo code re-installed
  
  after making additions and enhancements (in the ISR which reads the incoming servo values,) i again inserted the same servo code to 
  run the fpv-tilt servo and began the fine tuning process for the PIDS and the best place to have the code actuate the servo.
  
  my testing did use expensive, shielded, precision servos to test the quality of the fpv camera's imagery for specific uses - this has gone well
  
  the servo quality will, of course have an effect on the camera image, but in these tests, using 1080p cameras, i used no vibration damping and achieved 
  very good fpv image/video quality - (servo + bracket on servo arm + camera clamped to bracket) (bracket milled from hi-density polyurethane)
  
  PID NOTES:
  it seems that the PID values which are presented in this source code run perfectly across several sizes of custom quadcopter - 
   250 (511g), 450 (900g), 500 (1010g) as noted above
  these quadcopters are 
  
  
  ---------------------------------- please note -------------------------------------------------------------
  
  
  The following statement and terms of use are included from the original source code from Joop Brokking's YMFC-AL Project


///////////////////////////////////////////////////////////////////////////////////////
//Terms of use
///////////////////////////////////////////////////////////////////////////////////////
//THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
//IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
//FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
//AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
//LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
//OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
//THE SOFTWARE.
///////////////////////////////////////////////////////////////////////////////////////
//Safety note
///////////////////////////////////////////////////////////////////////////////////////
//Always remove the propellers and stay away from the motors unless you 
//are 100% certain of what you are doing.
///////////////////////////////////////////////////////////////////////////////////////


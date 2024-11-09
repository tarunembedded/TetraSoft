--> Timer Functionality in STM32 Controller Calculations:
Example :
System clock = 480MHz
Timer Clock = 240MHz
Timer Prescalar = 24-1
Auto Reload Register(ARR) = 5
Prescalar/Timer Clock = 24/240 
                      = 1/10MHz 
                      = 0.1 * 10^-6 (power of -6)
                      = 0.1usec
                      = 0.5usec for 1 ADC sample (Only If we use Timer in ADC)            
If we take 1024 or 512 or 256 or 2048 ADC samples for FFT purpose
                      = 0.5usec * 1024 ADC samples
                      = 512usec (Time taking to collect 1024 ADC samples in STM32 controller)

--> How to calculate Sampling Rate by using a Timer ?
Sampling Rate = (Timer clock/Timer Prescalar)/ARR 
              = (240/24)/5
              = 10MHz/5
              = 2MHz
Sampling Frequency (fs) = Sampling Rate / 2
                        = 2MHz/2
                        1MHz (This means we can see frequencies upt 1MHz)

--> How to find ADC Sampling Rate without Timer
If we are not using Timer in ADC parameter settings,
ADC Conversion Time = ADC Bit-Resolution/ADC Clock
                    = 16-bit/75MHz
                    = 16/(75 * 10^6)    // Power of 6
                    = 213.33nanoseconds   This is the basic ADC Conversion Time for a Single ADC sample without considering Oversamppling
When we Enabled Oversampling in ADC Parameter configuration in .ioc, ADC will take multiple samples (Example: 64 or 32 or 16 or 8 or 4 etc) and then average it to 
improve the Resolution.
Effective ADC Conversion Time = 64 * ADC Conversion Time
                              = 64 * 213.33ns 
                              = 13.6microseconds
Sampling Rate for this ADC is,
Sampling Rate = 1/Effective ADC Conversion Time
              = 1/13.6microseconds
              = 1/(13 * 10^6) //Power of 6
              = 0.735(10 ^ -6) //Power of -6
              = 73.53KSPS (Kilo samples for Second)

--> Frequency Bin spacing concept for FFT in STM32 Controller
In .Ioc, 
System clock = 480MHz
Timer clock = 240MHz
Timer Prescalar = 240-1
Timer ARF = 5-1 
                            Tmer clock/ Presacalar
                            = 240/240
                            = 1MHz
              Sampling Rate = 1MHz/5
                            = 20KHz
Frequency bin spacing, Let us say If we take 512 ADC samples or bins,
      Frequency bin spacing = Sampling Rate/Total ADC bins
                            = 20KHz/512
                            = 39.0625Hz (~40Hz) (This means, Total we are doing FFT for 512 bins, 
                            But According the Nyquist Shannon Theorem, we can view upt half of the sampling Rate i.e., 10KHz and in that 10KHz, we have 255 bins.
                            The bin to bin spacing or distance is 39.0625Hz )
If we want 52nd bin, the we should have to do,
Freq bin Spacing * bin nummber = 39.0625 * 52
                               = 2031.25Hz or 2.03125KHz
Here, we have Total 512 bins, but we can view the frequency upto 255 bins.

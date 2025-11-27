# EXP 3 : IIR-CHEBYSHEV-FITER-DESIGN

## AIM: 

 To design an IIR Chebyshev filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc ;
close ;
wp=input('Enter the pass band frequency (Radians )= ' );
ws=input('Enter the stop band frequency (Radians )= ' );
alphap=input( ' Enter the pass band attenuation (dB)=' );
alphas=input( ' Enter the stop band attenuation(dB)=' );
T=input('Enter the Value of sampling Time=');
//Pre warping- Bilinear Transformation
omegap=(2/T)*tan(wp/2);
disp(omegap,'omegap=');
omegas=(2/T)*tan(ws/2);
disp(omegas,'omegas=');
//Order of the filter
N=acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)))/(acosh(omegas/omegap));
16
disp(N,'N=');
N=ceil(N);
disp(N,'Round off value of N=');

omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N)));
disp(omegac,'omegac=');
Epsilon = sqrt ((10^(0.1*alphap))-1);
disp(Epsilon,'Epsilon=');
[pols ,gn] = zpch1(N, Epsilon,omegap );
disp(gn,'Gain');
disp(pols,'Poles');
hs=poly(gn,'s','coeff')/real(poly(pols,'s'));
disp(hs,'Analog Low pass Chebyshev Filter Transfer function');11
z=poly(0,'z');//Defining variable z
Hz=horner(hs,(2/ T)*((z -1)/(z+1)))// Bilinear Transformation
disp(Hz,'Digital LPF Transfer function H(Z)=');
HW=frmag(Hz,512); // Frequency response

w=0:%pi/511:%pi ;
plot(w/%pi,abs(HW));
xlabel(' Normalized Digital Frequency w');
ylabel('Magnitude ');
title(' Frequency Response of Chebyshev IIR LPF');




   

  
```
## PROGRAM (HPF): 
```
clc;
clear;
close();

// --- User Inputs ---
wp = input("Enter the pass band frequency (Radians)= ");   
ws = input("Enter the stop band frequency (Radians)= ");   
alphap = input("Enter the pass band attenuation (dB)= ");
alphas = input("Enter the stop band attenuation (dB)= ");
T = input("Enter the sampling time= ");

// -------------------------
// Pre-warp (bilinear)
// -------------------------
omegap = (2/T) * tan(wp/2);
omegas = (2/T) * tan(ws/2);

// -------------------------
// Chebyshev order (analytical)
// -------------------------
N = acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))) / acosh(omegas/omegap);
N = ceil(N);

// -------------------------
// Cutoff & epsilon
// -------------------------
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
Epsilon = sqrt((10^(0.1*alphap)) - 1);

// -------------------------
// Poles & gain from zpch1
// -------------------------
[pols, gn] = zpch1(N, Epsilon, omegap);

// -------------------------
// Display intermediate values
// -------------------------
mprintf("\nomegap= %f\n", omegap);
mprintf("omegas= %f\n", omegas);
mprintf("N= %f\n", N);
mprintf("Round off value of N= %d\n", N);
mprintf("omegac= %f\n", omegac);
mprintf("Epsilon= %f\n", Epsilon);
mprintf("Gain %f\n\n", gn);

mprintf("Poles\n");
for i = 1:length(pols)
    p = pols(i);
    if imag(p) >= 0 then
        mprintf(" - %g + %gi\n", real(p), imag(p));
    else
        mprintf(" - %g - %gi\n", real(p), abs(imag(p)));
    end
end
mprintf("\n");

// -------------------------
// Build analog Low-Pass H(s) (internal, not printed)
// -------------------------
s = poly(0, 's');  
hs_LP = gn / real(poly(pols, 's')); // analog LPF

// -------------------------
// Convert LPF -> HPF
// -------------------------
hs_HP = horner(hs_LP, omegac^2 / s); // LP -> HP transform
mprintf("\nAnalog High-Pass Chebyshev Transfer function H_HP(s)=\n\n");
disp(hs_HP);

// -------------------------
// Bilinear transform -> Digital HPF H(z)
// -------------------------
z = poly(0, 'z'); 
Hz_HP = horner(hs_HP, (2/T)*((z-1)/(z+1))); // s -> z
mprintf("\nDigital High-Pass Transfer function H_HP(z)=\n\n");
disp(Hz_HP);

// -------------------------
// Frequency response
// -------------------------
HW_HP = frmag(Hz_HP, 512);
w = 0:%pi/511:%pi;

figure();
plot(w/%pi, abs(HW_HP));
xlabel("Normalized Digital Frequency w");
ylabel("Magnitude");
title("Frequency Response of Chebyshev IIR High-Pass Filter");
xgrid();


   
```


## OUTPUT (LPF) : 

<img width="758" height="598" alt="image" src="https://github.com/user-attachments/assets/805e89c6-6af6-4664-83d2-786cf309978f" />


## OUTPUT (HPF) : 

<img width="762" height="597" alt="image" src="https://github.com/user-attachments/assets/5289c019-9141-40eb-9733-886eee806313" />



## RESULT: 
Thus,the IIR Chebyshev filter is verified

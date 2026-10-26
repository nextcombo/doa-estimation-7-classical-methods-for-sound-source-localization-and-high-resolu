# [DOA Estimation]7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]

clc\nclear all\nclose all\n​\n%% ------------------------------Initialize constants-------------------------------%\nc = 334;   % Sound speed c\nfs = 1000;   % Sampling frequency fs\nT = 0.1;   % ??\nt = 0:1/fs:T;  % Time [0, 0.1]\nL = length(t); % Time Length: 101\nf = 500;   % Interesting frequency\nw = 2*pi*f;  % Angular frequency\nk = w/c;   % wave number k\n​\n%% -------------------------------Each Element Coordinates------------------------------- %\nM = 17;   % Number of elements\n% Nmid = 12;      % reference point\n% d = 3;         % element spacing\n% m = (0:1:M-1) \nyi = zeros(M,1); % Generate a M*1 dimension zero matrix\nzi = [ 0; 3; 6; 9;12;15;18;21;24;12;12;12;12;12;12;12;12];\nxi = [12;12;12;12;12;12;12;12;12; 0; 3; 6; 9;15;18;21;24];\n%xi = xi.'      % Column vector m*d Number of elements*Element spacing\n​\n​\nfigure(1)\nplot(xi,zi,'r*');\ntitle('Cross-shaped microphone array')\n​\n​\n%% ---------------------------- Source of sound------------------------------------%\nx1 = 12;\ny1 = 10;\nz1 = 12; %Source location (12,10,12) x,z are horizontal plane\n \nx2 = 12;  % array center\ny2 = 0;\nz2 = 12;\n \nRic1 = sqrt((x1-xi).^2+(y1-yi).^2+(z1-zi).^2); % The distance from the sound source to each element\nRic2 = sqrt((x1-x2).^2+(y1-y2).^2+(z1-z2).^2); %sound to array center:10\nRn1 = Ric1 - Ric2; %The vector of sound path differences from the source to each element and reference elements\n​\ns1 = cos(2*w*t); % The vector received by the reference element\nAm = 10^(-1); % Amplitude\nn1 = Am * (randn(M, L) + j*randn(M, L)); % Gaussian white noise at each element\np1 = zeros(M,L);\n​\n​\n%% ----------------------------Each element's delay sum----------------------------------%\n% The most critical part of the program, delay summation, while simultaneously obtaining the acoustic pressure signal matrix and covariance matrix received by each element.\nfor k1 = 1:M\n    p1(k1,:) = Ric2/Ric1(k1) * s1.*exp(-j*w*Rn1(k1)/c);\n    % Received signal\nend\n​\np = p1+n1;  % The acoustic pressure signal matrix received by each element\nR = p*p'/L; % The autocovariance matrix of the received data is A.' which is a general transpose, and A' is its conjugate transpose.\n​\n​\n%% ----------------------------------Scanning Range----------------------------------%\n% We set the step size to 0.1, scan a 20x20 plane with double for loops to obtain M*1 vector matrices, and finally get the cross spectrum matrix (cross spectrum matrix)\n% According to DSP theory, this is the power of sound.\nstep_x = 0.1;  % The step is set to 0.1\nstep_z = 0.1;\ny = y1;\nx = (9:step_x:15);  % Scanning range 9-15\nz = (9:step_z:15); \n​\nfor k1=1:length(z)\n    for k2=1:length(x)\n        Ri = sqrt((x(k2)-xi).^2+(y-yi).^2+(z(k1)-zi).^2);  % The focus distance vector from this scanning point to each element\n        Ri2 = sqrt((x(k2)-x2).^2+(y-y2).^2+(z(k1)-z2).^2);  % 10.8628\n        Rn = Ri-Ri2;   % Scan points to the vector of phase difference between each element and reference elements\n        b = exp(-j*w*Rn/c); % Sound pressure focusing direction vector\n        Pcbf(k1,k2) = abs(b'*R*b); % CSM, the most critical, (1,18)*(18,18)*(18,1)\n    end\nend\n​\n​\n%% -------------------------------------Normalization-------------------------------------%\nfor k1 = 1:length(z)\n    pp(k1) = max(Pcbf(k1,:)); % The maximum value of the k1th element in Pcbf\nend\n​\nPcbf = Pcbf/max(pp);  % Normalize the amplitude of all elements by dividing it by its maximum value, (61,61)\n​\n​\n%% -------------------------------------Graphic Display-------------------------------------%\nfigure(2)\nsurf(x,z,Pcbf);\nxlabel('x(m)'),ylabel('z(m)')\ntitle('3D mono-source image')\ncolorbar\n \nfigure(3)\npcolor(x,z,Pcbf);\nshading interp;\nxlabel('x(m)');\nylabel('z(m)');\ntitle('Mono-source image')\ncolorbar\n​\n  
MUSIC, ESPIRIT, MVDR algorithms
Root-MUSIC algorithm
GCC-PHAT, SRP-PHAT algorithms

## Images

![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](01.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](02.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](03.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](04.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](05.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](06.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](07.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](08.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](09.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](10.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](11.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](12.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](13.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](14.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](15.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](16.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](17.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](18.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](19.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](20.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](21.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](22.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](23.jpg)
![(DOA Estimation)7 Classical Methods for Sound Source Localization and High-Resolution Spectral Estimation [with MATLAB Code]](24.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->

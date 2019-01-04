##mdct1变换

```
[flen,fnum] = size(x);
% Make column if it's a single row
if (flen==1)
    x = x(:);
    flen = fnum;
    fnum = 1;
end
% Make sure length is even
if (rem(flen,2)~=0)
    error('MDCT is defined only for even lengths.');
end
% We need these for furmulas below
N  = flen;    % Length of window
M  = N/2;     % Number of coefficients
N0 = (M+1)/2; % Used in the loop
% Frame loop
for i=1:fnum
    % Coefficient loop
    for k=0:(M-1)
        % Initialize
        y(k+1,i) = 0;
        % Sample loop
        for n=0:(N-1)
            y(k+1,i) = y(k+1,i) + x(n+1,i)*cos(pi*(n+N0)*(k+0.5)/M);
        end
    end
end
```

## IMDCT1变换

```
[flen,fnum] = size(x);
% Make column if it's a single row
if (flen==1)
    x = x(:);
    flen = fnum;
    fnum = 1;
end

% We need these for furmulas below
M  = flen;    % Number of coefficients
N  = 2*M;     % Length of window
N0 = (M+1)/2; % Used in the loop
N4 = N/4;     % Do we really need the division by N/4 ?

% Frame loop
for i=1:fnum
    % Sample loop
    for n=0:(N-1)
        % Initialize
        y(n+1,i) = 0;
        % Coefficient loop
        for k=0:(M-1)
            y(n+1,i) = y(n+1,i) + x(k+1,i)*cos(pi*(n+N0)*(k+0.5)/M)/N4;
        end
    end
end
```

##MDCT2

```

function y = mdct4(x)
% MDCT4 Calculates the Modified Discrete Cosine Transform
%   y = mdct4(x)
%
%   Use either a Sine or a Kaiser-Bessel Derived window (KBDWin)with 
%   50% overlap for perfect TDAC reconstruction.
%   Remember that MDCT coefs are symmetric: y(k)=-y(N-k-1) so the full
%   matrix (N) of coefs is: yf = [y;-flipud(y)];
%
%   x: input signal (can be either a column or frame per column)
%      length of x must be a integer multiple of 4 (each frame)     
%   y: MDCT of x (coefs are divided by sqrt(N))
%
%   Vectorize ! ! !

% ------- mdct4.m ------------------------------------------
% Marios Athineos, marios@ee.columbia.edu
% http://www.ee.columbia.edu/~marios/
% Copyright (c) 2002 by Columbia University.
% All rights reserved.
% ----------------------------------------------------------

[flen,fnum] = size(x);
% Make column if it's a single row
if (flen==1)
    x = x(:);
    flen = fnum;
    fnum = 1;
end
% Make sure length is multiple of 4
if (rem(flen,4)~=0)
    error('MDCT4 defined for lengths multiple of four.');
end

% We need these for furmulas below
N     = flen; % Length of window
M     = N/2;  % Number of coefficients
N4    = N/4;  % Simplify the way eqs look
sqrtN = sqrt(N);

% Preallocate rotation matrix
% It would be nice to be able to do it in-place but we cannot
% cause of the prerotation.
rot = zeros(flen,fnum);

% Shift
t = (0:(N4-1)).';
rot(t+1,:) = -x(t+3*N4+1,:);
t = (N4:(N-1)).';
rot(t+1,:) =  x(t-N4+1,:);
clear x;

% We need this twice so keep it around
t = (0:(N4-1)).';
w = diag(sparse(exp(-j*2*pi*(t+1/8)/N)));

% Pre-twiddle
t = (0:(N4-1)).';
c =   (rot(2*t+1,:)-rot(N-1-2*t+1,:))...
   -j*(rot(M+2*t+1,:)-rot(M-1-2*t+1,:));
% This is a really cool Matlab trick ;)
c = 0.5*w*c;
clear rot;

% FFT for N/4 points only !!!
c = fft(c,N4);

% Post-twiddle
c = (2/sqrtN)*w*c;

% Sort
t = (0:(N4-1)).';
y(2*t+1,:)     =  real(c(t+1,:));
y(M-1-2*t+1,:) = -imag(c(t+1,:));

```

## IMDCT

```

function y = imdct4(x)
% IMDCT4 Calculates the Modified Discrete Cosine Transform
%   y = imdct4(x)
%
%   x: input signal (can be either a column or frame per column)
%   y: IMDCT of x
%
%   Vectorize ! ! !

% ------- imdct4.m -----------------------------------------
% Marios Athineos, marios@ee.columbia.edu
% http://www.ee.columbia.edu/~marios/
% Copyright (c) 2002 by Columbia University.
% All rights reserved.
% ----------------------------------------------------------

[flen,fnum] = size(x);
% Make column if it's a single row
if (flen==1)
    x = x(:);
    flen = fnum;
    fnum = 1;
end

% We need these for furmulas below
N     = flen;
M     = N/2;
twoN  = 2*N;
sqrtN = sqrt(twoN);

% We need this twice so keep it around
t = (0:(M-1)).';
w = diag(sparse(exp(-j*2*pi*(t+1/8)/twoN)));

% Pre-twiddle
t = (0:(M-1)).';
c = x(2*t+1,:) + j*x(N-1-2*t+1,:);
c = (0.5*w)*c;

% FFT for N/2 points only !!!
c = fft(c,M);

% Post-twiddle
c = ((8/sqrtN)*w)*c;

% Preallocate rotation matrix
rot = zeros(twoN,fnum);

% Sort
t = (0:(M-1)).';
rot(2*t+1,:)   = real(c(t+1,:));
rot(N+2*t+1,:) = imag(c(t+1,:)); 
t = (1:2:(twoN-1)).';
rot(t+1,:) = -rot(twoN-1-t+1,:);

% Shift
t = (0:(3*M-1)).';
y(t+1,:) =  rot(t+M+1,:);
t = (3*M:(twoN-1)).';
y(t+1,:) = -rot(t-3*M+1,:);

```


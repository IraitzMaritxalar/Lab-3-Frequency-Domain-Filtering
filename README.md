# 🧪 Lab 3 – Frequency-Domain Filtering with fft2

**Course:** Mathematical Algorithms (DSP) — Image Processing Labs  
**Objective:** Explore frequency-domain filtering, low-pass and high-pass filters, and compare spatial vs frequency-domain convolution.

---

## 🔧 **0) Load image**
We use **peppers.png** (or **cameraman.tif** as fallback).  
Convert the image to grayscale and normalize to **double [0,1]**.

**Expected results:**
- Grayscale image  
- Pixel values between 0 and 1

---

## 🎯 **1) Magnitude spectrum (log-scale)**

F = fft2(I);
Fshift = fftshift(F);
S = log(1 + abs(Fshift));

yaml
Copiar código

**Effect:**
- Shows the **frequency content** of the image.  
- `fftshift` centers the zero-frequency component for better visualization.  
- Logarithm enhances visibility of low-magnitude frequencies.

---

## 🟢 **2) Ideal & Gaussian Low-pass filters**

D = sqrt(u.^2 + v.^2);
H_ideal_LP = double(D <= D0);
H_gauss_LP = exp(-(D.^2)/(2*sigma^2));

yaml
Copiar código

**Observations:**
- **Ideal LP:** sharp cutoff in frequency → ringing in spatial domain (Gibbs phenomenon).  
- **Gaussian LP:** smooth frequency response → avoids ringing, more natural blur.  

---

## 🔺 **3) Apply LP filters in frequency domain**

G_ideal = ifft2(ifftshift(H_ideal_LP .* Fshift));
G_gauss = ifft2(ifftshift(H_gauss_LP .* Fshift));

yaml
Copiar código

**Effect:**
- Ideal LP shows **ringing artifacts** around edges.  
- Gaussian LP gives a smooth blurred image.  
- Confirms the **convolution theorem**: frequency multiplication ↔ spatial convolution.

---

## 🖤 **4) High-pass via complement**

H_gauss_HP = 1 - H_gauss_LP;
G_hp = real(ifft2(ifftshift(H_gauss_HP .* Fshift)));

yaml
Copiar código

**Effect:**
- Extracts high-frequency details (edges, textures).  
- Gaussian HP = complement of Gaussian LP.  
- Values are scaled using `mat2gray` for display.

---

## 🔄 **5) Compare spatial vs frequency-domain Gaussian LP**

g1d = fspecial('gaussian',[1 7], 1.2);
I_spatial_gauss = imfilter(I, g1d'*g1d, 'replicate');

markdown
Copiar código

**Observations:**
- Spatial Gaussian LP and frequency-domain Gaussian LP give **almost identical results**.  
- Confirms **FFT-based filtering** is equivalent to spatial convolution.  
- Frequency-domain filtering is more efficient for large kernels.

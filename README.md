# 🎧 Audio Signal Recovery using DSP

##  Problem Statement

An audio signal was heavily distorted and noisy. The objective was to recover the original embedded audio using signal processing techniques.

---

##  Method

1. Loaded the noisy audio signal
2. Converted it into frequency domain using FFT
3. Observed frequency spectrum to identify noise
4. Designed a Butterworth Low Pass Filter
5. Applied filter to remove unwanted frequencies
6. Reconstructed clean signal

---

##  Results

###  Noisy Signal

![Noisy](noisy.png)

###  Frequency Spectrum

![Spectrum](spectrum.png)

###  Filtered Signal

![Filtered](filtered.png)

---

##  Output Audio

* Noisy audio → `noisy_audio.wav`
* Clean audio → `clean_audio.wav`

---

##  Observations

* Noise was dominant in high-frequency region
* Filtering reduced noise significantly
* Slight smoothing observed due to filtering

---

##  Conclusion

This experiment demonstrates how frequency-domain analysis helps isolate noise and recover useful information effectively using DSP techniques.

---

##  Note

Multiple cutoff frequencies were tested experimentally to achieve the best balance between noise removal and signal clarity.

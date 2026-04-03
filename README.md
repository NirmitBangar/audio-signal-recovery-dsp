import numpy as np
import matplotlib.pyplot as plt
from scipy.io import wavfile
import soundfile as sf
import os

# ================================
#  Setup folders
# ================================
os.makedirs("output", exist_ok=True)
os.makedirs("plots", exist_ok=True)

# ================================
#  Load Audio
# ================================
fs, signal = wavfile.read("input.wav")

# Normalize signal
signal = signal / np.max(np.abs(signal))

# ================================
#  Plot Functions
# ================================
def plot_time(signal, fs, title, path):
    t = np.arange(len(signal)) / fs
    plt.figure()
    plt.plot(t, signal)
    plt.title(title)
    plt.xlabel("Time (s)")
    plt.ylabel("Amplitude")
    plt.savefig(path)
    plt.close()

def plot_frequency(signal, fs, title, path):
    X = np.fft.fft(signal)
    freq = np.fft.fftfreq(len(signal), d=1/fs)

    plt.figure()
    plt.plot(freq, np.abs(X))
    plt.title(title)
    plt.xlabel("Frequency (Hz)")
    plt.ylabel("Magnitude")
    plt.savefig(path)
    plt.close()

def plot_spectrogram(signal, fs, title, path):
    plt.figure()
    plt.specgram(signal, Fs=fs)
    plt.title(title)
    plt.xlabel("Time")
    plt.ylabel("Frequency")
    plt.savefig(path)
    plt.close()

# ================================
#  FFT Analysis
# ================================
X = np.fft.fft(signal)
freq = np.fft.fftfreq(len(signal), d=1/fs)

# ================================
#  Low Pass Filter
# ================================
def low_pass_filter(signal, fs, cutoff):
    X = np.fft.fft(signal)
    freq = np.fft.fftfreq(len(signal), d=1/fs)

    H = np.abs(freq) < cutoff
    Y = X * H

    return np.real(np.fft.ifft(Y))

# ================================
#  Wiener Filter
# ================================
def wiener_filter(signal, noise_estimate):
    S = np.fft.fft(signal)
    N = np.fft.fft(noise_estimate)

    H = (np.abs(S)**2) / (np.abs(S)**2 + np.abs(N)**2 + 1e-10)
    Y = S * H

    return np.real(np.fft.ifft(Y))

# ================================
#  BEFORE FILTERING
# ================================
plot_time(signal, fs, "Time Domain (Before)", "plots/time_before.png")
plot_frequency(signal, fs, "Frequency Domain (Before)", "plots/freq_before.png")
plot_spectrogram(signal, fs, "Spectrogram (Before)", "plots/spec_before.png")

# ================================
# ⚙️ Filtering Process
# ================================

# Step 1: Low-pass filtering
cutoff = 4000  #  adjust based on your signal
filtered_lp = low_pass_filter(signal, fs, cutoff)

# Step 2: Noise estimation
noise_estimate = signal - filtered_lp

# Step 3: Wiener filtering
filtered_final = wiener_filter(signal, noise_estimate)

# ================================
#  AFTER FILTERING
# ================================
plot_time(filtered_final, fs, "Time Domain (After)", "plots/time_after.png")
plot_frequency(filtered_final, fs, "Frequency Domain (After)", "plots/freq_after.png")
plot_spectrogram(filtered_final, fs, "Spectrogram (After)", "plots/spec_after.png")

# ================================
#  Save Output
# ================================
sf.write("output/recovered.wav", filtered_final, fs)

print(" Done! Check 'output/' and 'plots/' folders.")

Below there are multiple explanations of "cross-correlation". They are ordered from most intuitive and imprecise, to most correct and opaque.

{drawing of sine-y spike rate signals, multiplied}

Signal correlation is found by multiplying two signals sample-wise together, and then calculating how 'high' the resulting signal is on average:

$$\text{corr}_{x,y} = \frac{1}{N} \sum_i x[i] \cdot y[i]$$

where the index $i$ runs over all samples of $x$ and $y$.
$N$ is how many samples either signal has.

Cross-correlation (CC, or xcorr) is calculated the same way, except that first, one signal has been shifted in time by some number of samples $τ$. Symbolically, time-shifting corresponds to indexing with an offset. Hence:

$$\text{xcorr}_{x, y} = \frac{1}{N} \sum_i x[i] \cdot y[i-τ]$$

Because of the time shift $τ$, there are indices $i$ for which one signal has no corresponding sample in the other signal. To remedy this, we can extend both signals by padding them with their last/first value.
Alternatively, we can ignore those non-overlapping samples. Because the sum will then run over less samples, we need to correct the averaging denominator:

$$\text{xcorr}_{x, y} = \frac{1}{N-τ} \sum_i x[i] \cdot y[i-τ]$$

This is sometimes called the *unbiased* CC estimator.

Cross-correlation is often plotted as a function of the time offset $τ$.

How do we calculate the cross-correlation for spike trains, which are lists of spike times, and not 'real signals'? By converting them to real signals; namely: rate signals. We either:
- bin the spike trains in time, so that each bin (i.e. sample) counts the number of spikes that occured within it; or:
- convolve the spike trains with some continuous kernel, to calculate a temporal density estimate. The resulting signals, if normalised correctly, will have units of "spikes per second". You then use either the continous definiton of cross-correlation: $\text{xcorr}_{x, y} = \frac{1}{T-τ} \int x(t) \cdot y(t-τ) \cdot dt$; or you discretise the density signals.
 
To complete the classic signal processing story on CC, we must note that (1) calculating the cross-correlation of $x$ and $y$ for the entire range of time shifts $τ$ is the same as convolving $x$ with at time-reversed version of $y$; and that (2) this means that the full $\text{xcorr}_{x, y}(τ)$ function can be calculated efficiently by Fourier-transforming $x$ and $y$, multiplying them sample-wise in the frequency domain, and transforming back to the time domain. This method costs $O(N \log N)$ multiply-add operations, instead of the $O(N^2)$ operations for calculating a full cross-correlation (or convolution) directly in the time domain.[^a]
Note that these efficiency considerations don't matter for what we do. They're only included here for completeness

[^a]: The reason for this efficiency gain is the Fast Fourier Transform (FFT), an algorithm to calculate discrete Fourier transforms in $O(N \log N)$ operations, instead of the $O(N^2)$ required when  translating the symbolic  definition of the discrete Fourier transform directly.

Implementations of cross-correlation in scientific programming packages:
- [`scipy.signal.correlate` in Python](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.correlate.html)
- [`xcorr` in MATLAB](https://uk.mathworks.com/help/matlab/ref/xcorr.html)
This is a high-level overview of different methods used in network inference. In other words, these are algorithms that take as input activity signals of individual [[node]]s, and return a [[connectivity matrix]].


## Cross-correlation

One of the simplest methods is [[cross-correlation]]. That is: calculate to what extent a pair of signals matches up after one of them has been shifted in time with respect to the other. If the signal of node B looks like that of node A, shifted a bit forward in time, then we infer that A connects to B.
Cross-correlation is a good baseline method. One problem with it, for example in neural networks, are inhibitory connections. If neuron A inhibits neuron B, then B will be silent after A is active. The activity signals will then not look similar even after a time shift. Correlation can only pair up signals that are generally high when the other is high, and negative when the other is negative.


## Pattern counting

Another approach is what I call "pattern counting". It encompasses different so called *[[information theoretic]]* measures, generally with "entropy" in their name: [[transfer entropy]], [[joint entropy]], [[mutual information]], and [[causation entropy]]. They operate on time-binned event data; in the example below these are time-binned spike trains.

The [[information theoretic]] algorithms look at all possible patterns that can occur in a given "window", where that window is for example: the number of spikes of neuron 1 at any time $t$, and the number of spikes of neuron 2 a fixed number of samples later (say $τ$ samples later, so at time-bin $t+τ$).
Say that the neurons never spike more than once per time-bin. The only possible patterns are then: $[0, 0]$, $[0, 1]$, $[1, 0]$, and $[1, 1]$. The algorithm will first count the occurences of each of these patterns. It will then infer a connection if one pattern occurs more often than would be expected from the average occurence of these patterns over the entire neuron population.
If for example we suspiciously often get the pattern $[1, 0]$ for neurons A and B respectively, then there might well be an inhibitory connection from A to B.

So unlike [[cross-correlation]], the [[information theoretic]] algorithms can detect inhibitory connections. This is because they are agnostic to the actual signal values in the patterns; a "$0$" has the same significance as a "$1$" or a "$2$". CC on the other hand is proportional to the signal values: the higher the signal magnitude, the higher the CC.

The two big problems of the [[information theoretic]] methods are that they are (1) quite sensitive to the bin-width and hence not always very robust; and (2) data-intensive. The more bins you look at in your window, and the more values are allowed in each bin, the more possible patterns can be made. In fact, the number of patterns grows exponentially in the number of samples.[^pat]

[^pat]: A window $[x_1, x_2, .., x_S]$ with $S$ samples, where each sample can take on $V$ values (eg $0, 1, 2, .., V-1$), can take on $V^S$ different patterns. For binary data and 8 samples that is e.g. 256 patterns.

Naively applying the pattern counting methods to voltage traces would therefore not be a good idea: each time-bin can take on many values (depending on how you quantize the ± 100 mV range of a neuron's membrane potential); and it is not clear what samples we should include in our 'window'. To be comprehensive, we would choose a large window, to capture interactions with different time delays. The result would be an unfathomably large amount of possible patterns, the majority of which  not actually occurring in our data. The patterns that do occur will not be distinguishable from noise.

{difference between joint ent, transfer ent, "generalised transfer ent", "extended transfer ent", causation ent, MI, ..}

## Indirect effects & global bursts

{Partial correlation & global regul, via Sutera i.e. NCC winners}
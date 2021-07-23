A system we cannot 'see inside of' generates data that we can see. Can we know what the system is like inside, from only this data? This is the inverse problem.

## Examples

A common example is X-ray computed tomography (CT): reconstructing a 3D view of a body from 2D projections.

Another example is how to [[reconstruct synaptic connectivity in the brain by only looking at neurons' somatic voltages]].

In both cases, we have access to data that is causally dependent on the system. The shapes on a flat X-ray scan depend on the imaged object. The voltages of the measured neurons depend (inter alia) on how the neurons are connected.[1]

[1] The direction of causality is not as clear in this case, though. On timescales longer than a typical neural recording session, neural voltages also influence the network structure. "Neurons that fire together, wire together". Biology reifies temporal associations with new neural wiring. (With the direction of connection dictated by temporal precedence: a physical imprint of presumed cause and effect).

From this causally dependent data, can we know something about the generating system?

## Loose formalism

If you have a general idea ('forward' model $F$) of how the system generates observed data, based on unknown parameters that instantiate the model -- i.e.
$$\text{data} = F(\text{parameters})$$
.. then the inverse problem is instantiating the model, i.e. finding the parameters from the data, i.e. finding $F^{-1}$.
This is also called 'system identification'.


---
Inspired by https://en.wikipedia.org/wiki/Inverse_problem

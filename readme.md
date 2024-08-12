# Horse Race Modeling for Fingerpress Tasks

Horse Race modeling implementation for sequential fingerpress tasks:


The model follows the following formulation: For each fingerpress, 5 horses (corresponding to five fingers) are competeing againts each other to reach a threshold. Whichever reaches the threshold first, is gonna be decided to be pressed. Different dependencies can be assumed between each of these races. For now we have assumed that competition processes between horses in different races are *independent* while competition processes between horses in the same race can be dependant. The only dependency between races are that they are sharing a common planning resource, let it be visual or motor. For both we tested different resource sharing strategies that is dicussed in the following. We are assuming evidence for each horse in each race is determined by a linear dynamical system with inputs ($X^{n+1}_{5\times L} = A_{5 \times 5}  \times X^{n}_{5 \times L} + U^{n}_{5 \times L} + \epsilon$ 
). The number of possible visual processes were set to 3, and for motor either 1, 3, or 4(the last model) depending on the modeling approach. We decided to fix the planning capacity to be more naturally plausible. 


Description of different models and the optimization approach:


As there were many parameters avaialble in the model to fit the data, we followed the following logic to constrain the space of parameters:

1- Fitting a single press reaction time and accuracy in a force reaction time task paradigm. A learned press is akin to a *congruent* stimuli in a Flanker task. A random press is akin to a *Neutral* stimuli and a changed press is akin to an *incongruent* stimuli in a flanker task. Using this, we fitted the self and lateral inhibition parameters (Matrix A), and total visual and motor planning parameters.

2- Moving to sequences, we first used random sequence production data to fit visual planning capacity sharing. All optimizations were performed using Particle Swarm Optimization because local optimization algortithms were unsuccessful in this problem. A fixed total visual planning capacity could not be able to replicate the data, leading to a model with total visual capacity extension when more visual information is available (model_seq_inhibition_capacity_extension_fit.ipynb)

3- We used learned and learned changed sequences data to fit motor planning capacity sharing and adaptive coefs of the visual and motor modules. Here we assumed that the total motor planning capacity is fixed. Using motor planning to only influnce the current press was not effective at replicating the effect of decreasing slow down at the press of the change seen in the emperical data (model_seq_specific_capacity_extension_fit.ipynb)

Moving to capacity sharing in motor planning with a fixed total capacity, we were able to replicate the decrease in slowdown at the position of the change (both time and accuracy). But it was not able to replicate the effect of post change slowdown. (model_seq_specific_capacity_extension_both_fit.ipynb)

We tested memory adaptive coef along with visual adaptive coef to solve the post change slowdown, but were unsuccessful. (model_seq_specific_capacity_extension_both_memory_adaptive_fit.ipynb)

As the last resort, we let motor planning window to be 4 (one more than the visual planning window). This approach did not solve the problem as well (model_seq_inhibition_different_capacity_extension_both_fit.ipynb)
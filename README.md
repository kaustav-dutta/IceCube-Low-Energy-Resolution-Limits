# IceCube-Low-Energy-Resolution-Limits
Resolution limits for low energy reconstruction in IceCube Upgrade (1-GeV-20GeV) &amp; DeepCore (5GeV-20GeV)

# Brief Description 
The study analyses the processes that limit the physics information in IceCube events and the contributions from these processes. These include the transverse shower spread, in-ice scattering, module resolutions (due to finite number of PMTs) and poissonian noise in modules. The cumulative effect of these factors establishes resolution limits which are then compared with the benchmark algorithms: GNN-based Dynedge for ICU and FLERCNN/RETRO for DC. 

# Simulation
Principal photon generation and propagation simulated by Photon Progation Code (PPC) developed by Dima Chirkin. To switch on/off different processes the following changes have been made to the configuration files or invented:

a) Switch off scattering: The second column in icemodel.dat, which sets the scattering coefficient, must be made arbitrarily small. Note that this cannot be fixed to 0 (causes simulation inefficiencies). 

b) Switch off transverse spread: The angular distribution of shower particles relative to the secondary charged particle given in the pro.cxx file. Excluding this part of the code switches off any shower spread. 

c) Switch on module resolutions: PPC output includes truth directional information of detected photons. An mDOM is simulated by creating 4 bands of PMTs with populations {5,7,7,5}. The angle between the nearest PMT and hit photon is calculated, and the photon is accepted/rejected based on rejection sampling using the acceptance curve informtion of the PMT. If accepted, PMT vector direction assumed as the photon direction.

d) Switch on module noise: PPC only simulates physics hits. To replace a physics hit with a noise hit, the plot of signal purity vs survivng hits after noise cleaning in the standard analysis chain is used. This is followed by rejection sampling based on the number of physics hits. To include a nloise hit spatially, a random PMT is triggered. To include a noise hit temporally, a noisy timestamp is sampled from an uniform distribution in the event time window. 

# Likelihood-based approach
In IceCube reconstructions, likelihood-based techniques are usually quite computationally expensive. Usually, we have a 7-dimensional paramter space: vertex position (x,y,z), vertex time (t), arrival angles (zenith, azimuth) and energy. In this study, the MC truth energy and vertex point and time is known. This reduces the dimensionally to 2. 

The direction and timing information of detected photons are used for reconstruction, followed by a combined approach. In the direction case, the likelihood of obtaining an angular distribution of photons relative to a hypothesis charged particle direction is explorted. In the timing case, the likelihood of obtaining a delay timing distribtution of photons given a hypothesis is used. But these distributions change depending on the event start position and direction of the charged particle. Therefore, a randomised geometry is constructed and events are simulated with vertices uniformly distributed throughout the detector. This kills any photon sampling bias by the fixed detector geometry and the distributions are now independent of the arrival directions. 

# Variable Bandwidth KDE
To fit the expected distributions, we use a variable bandwidth KDE. This technique is superior to other fitting techniques as it efficiently captures the high and low statistics of any given distribution without overfitting or underfitting. First, a window is constructed which slides over all the datapoints and the bandwidth of a gaussian kernel is computed depending on the population enclosed within the window. If the population is large, then the bandwidth is narrow and vice-versa. Finally, all individiual kernels are added up and normalised which gives the overall KDE. Note that there are 2 (or 3) parameters to be optimised: kernel bandwidth, and window size (1D or 2D). This optimisation is performed by analysing the disagreements between the underlying histogram populations and KDE estimates. A minimum overall diviation results in the optimisation.

# Idealistic Assumptions
There are a few idealistic simplifications in event simulation and detector response:
a) We use deep homogenous ice here which means optical properties stay constant with depth and within a layer, implying no anisotropies and birefringence. 

b) The geometry is approximately infinite to make sure all events are completely confined. This ensures no photon escapes the detector. All photons, unless absorbed, are detected. The horizonatal and vertical spacings for a given geometry, is also kept constant.

c) Only hadronic cascades are simulated which are easy to confine within the detector.

d) Some parts of the study assume uniform angular sensitivity of a module which gives access to the MC truth direction information of photons. 

e) Upgrade mDOM PMTs have same collection and quantum efficiencies as Gen-1 DOMs. 

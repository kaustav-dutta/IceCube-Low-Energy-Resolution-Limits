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

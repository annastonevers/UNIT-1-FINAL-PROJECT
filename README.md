

Comparison of Models:

IF models are one of the simplest neuron models. They integrate synaptic currents and fires a spike once the membrane potential reaches a predefined threshold. They then reset. These models lack a refractory period of a biological neuron as well as other chaacteristcs such as leakage and adaptation. 

LIF models improve upon this by incoorporating a leak in the membrane potential. So, if no imput is recieved, the potential will gradually return to resting state. This model is more biologically acurate but still lacks a refractory period and any ion channel dynamics.

A simple feedforward neural network has the ability to learn and adapt, unlike IF/LIF models. It responds directly to inputs. However, it is more difficult to make preductions compared to other biolgically inspired models. 

The LIF model contains more information that the IF model since it includes the leak. The IF model is most binary given its set pattern, making it more discrete and the most concise. The FNN is the most complex and contains the most bits since it is able to store information over time. 

{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# UNIT 1 FINAL PROJECT\n",
    "#### <u>Group 8: Annaston Evers, Juliette Vasquez, Madison Gaines, Mrityunjay Sivakumar, Shirley Lin, Uday Thakar, Victor Irby<u>\n",
    
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "# Parameters\n",
    "V_rest = -65  # Resting potential (mV)\n",
    "V_th = -55    # Threshold potential (mV)\n",
    "V_reset = V_rest  # Reset potential after firing (mV)\n",
    "tau = 20  # Membrane time constant (ms)\n",
    "R = 10  # Membrane resistance (MΩ)\n",
    "dt = 0.1  # Time step (ms)\n",
    "T = 100  # Total time for the simulation (ms)\n",
    "\n",
    "# Function to simulate the integrate-and-fire model\n",
    "def integrate_and_fire(I_ext, T=100, dt=0.1):\n",
    "    # Initialize time and membrane potential arrays\n",
    "    t = np.arange(0, T, dt)\n",
    "    V = np.full_like(t, V_rest)  # Membrane potential (initially resting potential)\n",
    "\n",
    "    # Iterate over each time step\n",
    "    for i in range(1, len(t)):\n",
    "        # Differential equation for membrane potential\n",
    "        dV = (- (V[i-1] - V_rest) + R * I_ext) * dt / tau\n",
    "        V[i] = V[i-1] + dV\n",
    "\n",
    "        # Check if the potential exceeds the threshold\n",
    "        if V[i] >= V_th:\n",
    "            V[i] = V_reset  # Reset after firing\n",
    "\n",
    "    return t, V\n",
    "\n",
    "# Simulate with different input currents (e.g., 2 levels of input current)\n",
    "I_ext_low = 1.5  # Low input current (nA)\n",
    "I_ext_high = 3.0  # High input current (nA)\n",
    "\n",
    "# Get results for both currents\n",
    "t_low, V_low = integrate_and_fire(I_ext_low)\n",
    "t_high, V_high = integrate_and_fire(I_ext_high)\n",
    "\n",
    "# Plotting the results\n",
    "plt.figure(figsize=(10, 6))\n",
    "\n",
    "# Low current input\n",
    "plt.subplot(2, 1, 1)\n",
    "plt.plot(t_low, V_low, label=f'I_ext = {I_ext_low} nA')\n",
    "plt.axhline(y=V_th, color='r', linestyle='--', label=\"Threshold (-55 mV)\")\n",
    "plt.axhline(y=V_rest, color='g', linestyle='--', label=\"Resting Potential (-65 mV)\")\n",
    "plt.title('Firing Pattern with Low Input Current')\n",
    "plt.xlabel('Time (ms)')\n",
        "plt.ylabel('Membrane Potential (mV)')\n",
    "plt.legend(loc='upper right')\n",
    "\n",
    "# High current input\n",
    "plt.subplot(2, 1, 2)\n",
    "plt.plot(t_high, V_high, label=f'I_ext = {I_ext_high} nA', color='orange')\n",
    "plt.axhline(y=V_th, color='r', linestyle='--', label=\"Threshold (-55 mV)\")\n",
    "plt.axhline(y=V_rest, color='g', linestyle='--', label=\"Resting Potential (-65 mV)\")\n",
    "plt.title('Firing Pattern with High Input Current')\n",
    "plt.xlabel('Time(ms)')\n",
    "plt.ylabel('Membrane Potential(mV)')\n",
    "plt.legend(loc='upper right')\n",
    "\n",
    "plt.tight_layout()\n",
    "plt.show()\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Visualization of Results\n",
    "The plots below illustrate the firing patterns of a neuron under low and high input current conditions.\n",
    "![IF Model Results](graph.jpg)\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Question 3\n",
    "**Looking at the above visualization / based on what you know regarding the LIF model -- what aspect of this model is most unlike a real biological neuron? What is missing? Additionally what aspects are like a biological neuron? Compare and Contrast the two.**\n",
    "\nIn terms of aspects missing in this graph, our model is missing a refractory period, action potentials, and leaky channels when compared to a real biological neuron. This model is similar to real neurons in the way that the response to input currents is non-linear voltage changes. Furthermore, there is also a resting membrane potential and appropriate membrane threshold that the neuron reaches to before dropping back to resting."
   ]
  },
  {

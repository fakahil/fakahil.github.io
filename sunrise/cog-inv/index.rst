.. title: Applying Center Of Gravity Method on the IMAX inverted profiles
.. slug: cog-inv
.. date: 2020-09-16 14:30:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text



This code computes the LOS magnetic field from Stokes I and V inverted profiles of IMAX data

.. code-block:: python

	import numpy as np
	import matplotlib.pyplot as plt
	import pyfits
	import math
	import os
	import sys
	from numpy import trapz

	lam_0 = 5250.208 #the central wavelength of the FeI line (in Angstroms)
	c_0 = 4.67*pow(10, -13) #A^-1 G^-1
	g = 3 # the effective Lande factor
	sigma = 3*pow(10, -3) 

	wav = np.linspace(5249.808,5250.808,51)
	del_lam = wav[10:26] - lam_0

	path= '/home/kahil_administrator/Desktop/Inversions_177/results/'
	path2 = '/home/kahil_administrator/Desktop/Inversions_177/B_cog_on_inv/'
	os.makedirs(path2)


	for n in range(18,58): 

	 dir_path = path+'Obs177_Cyc0'+str(n)+'/lev2.2_GlobSL0.120_936x936/'
	 im = pyfits.open(dir_path+'inverted_profs.1.fits',ignore_missing_end=True)
	 profs = im[0].data
	 B= np.zeros((936,936)) # LOS magnetic field
	 del_lam_G = np.zeros((936,936))

	 for i in range(936):
	  for j in range(936):
	   Prof_I = profs[i,j,0,:]
	   Prof_V = profs[i,j,1,:]
	   I_lam = Prof_I[10:26]
	   V_lam = Prof_V[10:26]
	   I_c =  Prof_I[10]       #Prof_I[0:10].max()
	   del_lam_G[i][j] = (np.trapz(V_lam*del_lam, x=del_lam))/(np.trapz(I_c-I_lam, x=del_lam))  
	   B[i][j] = 0.88*abs(del_lam_G[i][j]/(c_0*g*pow(lam_0,2))) 

	 hdu = pyfits.PrimaryHDU(B)
	 hdu.writeto(path2+'B_177_'+str(n)+'.fits')

 
 
 

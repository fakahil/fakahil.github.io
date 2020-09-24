.. title: Applying Center Of Gravity Method for the LOS field strength computation (L8)
.. slug: cog-l8
.. date: 2020-09-15 16:45:59 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

Below is a python code to compute the LOS magnetic field strength using the Center Of Gravity Technique. The method is applied on inverted IMAX data (cycle 177) in their current data format for the L8 observation mode (8 wavelengths within the iron line of IMAX):

.. code-block:: python

   

	import numpy as np
	import matplotlib.pyplot as plt
	import pyfits
	import math
	import os
	import sys
	from numpy import trapz

		path1 = '/home/kahil_administrator/Desktop/IMaX_data/L12.2/'
		path2 = "/home/kahil_administrator/Desktop/Inversions_177/B_COG_L_8/"
		os.makedirs(path2)

		lam_0 = 5250.2 #the central wavelength of the FeI line (in Angstroms)
		c_0 = 4.67*pow(10, -13) #A^-1 G^-1
		g = 3 # effective Lande factor
		sigma = 3*pow(10, -3) 

		## COG on 12 points: (only 10 excluding the two wavelength points):
		del_lam1 = np.arange(-122.5,157.5,35)
		for n in range(10,50):
			ind = '225_0'+str(n) 
			V_lam = np.zeros((936,936,8)) # V pixel array
			I_lam = np.zeros((936,936,8)) # I pixel array 
			del_lam_G = np.zeros((936,936)) # COG wavelength
			B= np.zeros((936,936)) # LOS magnetic field

			dir_path = path1+ind

			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m122.5_I.fits.gz')
			I_m122_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m122.5_V.fits.gz')
			V_m122_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m087.5_I.fits.gz')
			I_m87_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m087.5_V.fits.gz')
			V_m87_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m052.5_I.fits.gz')
			I_m52_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m052.5_V.fits.gz')
			V_m52_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m017.5_I.fits.gz')
			I_m17_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_m017.5_V.fits.gz')
			V_m17_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p017.5_I.fits.gz')
			I_p17_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p017.5_V.fits.gz')
			V_p17_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p052.5_I.fits.gz')
			I_p52_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p052.5_V.fits.gz')
			V_p52_5 = im[0].data


			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p087.5_I.fits.gz')
			I_p87_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p087.5_V.fits.gz')
			V_p87_5 = im[0].data

			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p122.5_I.fits.gz')
			I_p122_5 = im[0].data
			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p122.5_V.fits.gz')
			V_p122_5 = im[0].data

			im = pyfits.open(dir_path+'/reduc_rnr_'+ind+'_p192.5_I.fits.gz')
			I_c = im[0].data

			for i in range(936):
			for j in range(936):
			 I_lam[i][j] = np.array([I_m122_5[i][j], I_m87_5[i][j], I_m52_5[i][j], I_m17_5[i][j],I_p17_5[i][j],I_p52_5[i][j],I_p87_5[i][j],I_p122_5[i][j] ])
			 V_lam[i][j] = np.array([V_m122_5[i][j], V_m87_5[i][j], V_m52_5[i][j], V_m17_5[i][j],V_p17_5[i][j],V_p52_5[i][j],V_p87_5[i][j],V_p122_5[i][j] ])
			 del_lam_G[i][j] = (np.trapz(V_lam[i][j]*del_lam1, x=del_lam1))/(np.trapz(I_c[i][j]-I_lam[i][j], x=del_lam1))  
			 B[i][j] = abs(del_lam_G[i][j]*pow(10,-3)/(c_0*g*pow(lam_0,2))) 

			hdu = pyfits.PrimaryHDU(B)
			hdu.writeto(path2+str('/B_') + ind+'.fits')


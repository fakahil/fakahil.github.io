.. title: Visualizing the IMAX (Sunrise 2) data points and the corresponding inverted STOKES profiles by SPINOR
.. slug: stokes-2013
.. date: 2020-09-12 13:30:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text


This code illustrates for each pixel on any of the inverted data of Sunrise 2 (cycle 548) the inverted Stokes profiles and the IMAX data points.

.. code-block:: python


	import numpy as np
	import matplotlib.pyplot as plt
	import pyfits
	import math
	import os
	import sys
	lam_0 = 5250.208 # central wavelength


	ind = '018'
	p2  = '/home/kahil_administrator/Sunrise_2/IMaX/Spinor1D_inversions_2016_Feb/'
	dir_path = p2+'Obs548_Cyc'+ind+'/lev2.2_atlas_GlobSL0.250_936x936/'
	wd = '/home/kahil_administrator/kahil_data/project_2/for_thesis/'



	wav = (np.linspace(5249.808,5250.808,51) - lam_0)*1000
	del_lam2 = np.array([wav[14],wav[16],wav[18],wav[20],wav[22],wav[24],wav[26], wav[31]])


	#wav = np.arange(51) #wavelength points spanned by inversions or wav = np.linspace(5249.808,5250.808,51)



	############################# stray light corrected imax
	imax = pyfits.getdata(dir_path+'imax.fits')
	I_qs = (imax[:,:,0,7])[0:268,0:368].mean()

	################################### best fit profiles returned by inversions

	profs = pyfits.getdata(dir_path+'inverted_profs.1.fits')

	####################################################physical parameters returned by inversions
	inv_data = pyfits.getdata(dir_path+'inverted_atmos.fits')
	temp_logtm2p5 = inv_data[5,:,:]
	temp_logtm0p9 = inv_data[6,:,:]
	temp_logt0 = inv_data[7,:,:]
	B = inv_data[8,:,:] 
	Gamma = inv_data[9,:,:]
	Az = inv_data[10,:,:]
	V_LOS = inv_data[11,:,:]
	Mu = inv_data[12,:,:]
	B_LOS = inv_data[13,:,:]
	Chi = inv_data[14,:,:]


	
	#i,j=np.where((B_LOS > 1200) & (B_LOS <12100))    # scatter 
	i = int(raw_input('Enter x:'))
	j = int(raw_input('Enter y:'))

	I_lam = imax[i,j,0,:]/I_qs
	Q_lam = imax[i,j,1,:]/I_qs
	U_lam = imax[i,j,2,:]/I_qs
	V_lam= imax[i,j,3,:]/I_qs
	   



	Ip = I_lam+V_lam
	Im = I_lam-V_lam

	Prof_I = profs[i,j,0,:]
	Prof_V = profs[i,j,1,:]
	Prof_Q = profs[i,j,2,:]
	Prof_U = profs[i,j,3,:]

	Prof_Ip = Prof_I+Prof_V
	Prof_Im = Prof_I-Prof_V
	csfont = {'fontname':'Comic Sans MS'}
	fig = plt.figure(figsize=(20,12),facecolor='white')
	ax1 = fig.add_subplot(2,2,1)
	ax1.plot(del_lam2, I_lam, 'kx',ms=10,mew=2,label='IMaX')
	ax1.plot(wav,Prof_I,label='best fit')
	#ax1.plot(del_lam2,Ip,'rx',ms=10,mew=2)
	#ax1.plot(del_lam2, Im, 'bx',ms=10,mew=2)

	#ax1.plot(wav,Prof_Ip,'r')
	#ax1.plot(wav,Prof_Im,'b')
	ax1.set_xlabel('$\Delta\lambda$',fontsize=18)
	ax1.set_ylabel('Stokes I',fontsize=18)
	ax1.set_xticks(np.arange(-400,600,100))
	ax1.set_xticks(np.arange(-400,600,50),minor=True)
	ax1.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 12)
	ax1.tick_params(axis = 'both', which = 'minor', length=3, width=1)
	#ax1.set_title('$B_{LOS}$ = '+str("%.1f" %B_LOS[i][j])+' G',fontsize=20)
	#ax1.legend(loc='lower right')
	ax1.grid()
	ax1.legend(loc='lower left')

	ax2 = fig.add_subplot(2,2,2)
	ax2.plot(del_lam2, V_lam,'kx',ms=10,mew=2,label='IMaX')
	ax2.plot(wav, Prof_V, label='best fit')
	ax2.set_xlim(-400,600)
	ax2.set_xlabel('$\Delta\lambda$',fontsize=20)
	ax2.set_ylabel('Stokes V',fontsize=20,**csfont)
	ax2.set_xticks(np.arange(-400,600,100))
	ax2.set_xticks(np.arange(-400,600,50),minor=True)
	ax2.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 12)
	ax2.tick_params(axis = 'both', which = 'minor', length=3, width=1)
	#ax2.set_title('$B_{LOS}$ = '+str("%.1f" %B_LOS[i][j])+' G',fontsize=20)
	#ax2.legend(loc='lower right')
	ax2.grid()
	ax2.legend(loc='lower left')

	ax3 = fig.add_subplot(2,2,3)
	ax3.plot(del_lam2, Q_lam,'kx',ms=10,mew=2,label='IMaX')
	ax3.plot(wav, Prof_Q, label='best fit')
	ax3.set_xlim(-400,600)
	ax3.set_xlabel('$\Delta\lambda$',fontsize=20)
	ax3.set_ylabel('Stokes Q',fontsize=20,**csfont)
	ax3.set_xticks(np.arange(-400,600,100))
	ax3.set_xticks(np.arange(-400,600,50),minor=True)
	ax3.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 12)
	ax3.tick_params(axis = 'both', which = 'minor', length=3, width=1)
	#ax3.set_title('$B_{LOS}$ = '+str("%.1f" %B_LOS[i][j])+' G',fontsize=20)
	#ax2.legend(loc='lower right')
	ax3.grid()
	ax3.legend(loc='lower left')

	ax4 = fig.add_subplot(2,2,4)
	ax4.plot(del_lam2, U_lam,'kx',ms=10,mew=2,label='IMaX')
	ax4.plot(wav, Prof_U, label='best fit')
	ax4.set_xlim(-400,600)
	ax4.set_xlabel('$\Delta\lambda$',fontsize=20)
	ax4.set_ylabel('Stokes U',fontsize=20,**csfont)
	ax4.set_xticks(np.arange(-400,600,100))
	ax4.set_xticks(np.arange(-400,600,50),minor=True)
	ax4.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 12)
	ax4.tick_params(axis = 'both', which = 'minor', length=3, width=1)
	#ax4.set_title('$B_{LOS}$ = '+str("%.1f" %B_LOS[i][j])+' G',fontsize=20)
	#ax2.legend(loc='lower right')
	ax4.grid()
	ax4.legend(loc='lower left')

	plt.suptitle('$B_{LOS}$ = '+str("%.1f" %B_LOS[i][j])+' G',fontsize=24,**csfont)
	plt.savefig(wd+'pixel'+str(j)+','+str(i))
	plt.clf()









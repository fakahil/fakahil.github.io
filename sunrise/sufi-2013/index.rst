.. title: Visualizing the SUFI/sunrise 2 images at all wavelengths
.. slug: sufi-2013
.. date: 2020-09-25 20:35:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text



This code shows in one figure the SUFI images in all wavelengths for sunrise 2 (used for my sunrise 2 paper)

.. code-block:: python

	import pyfits
	import matplotlib.pyplot as plt
	import numpy as np
	import pylab
	from numpy import flipud as fl
	from mpl_toolkits.axes_grid1 import make_axes_locatable, axes_size

	n = 2 #sigma range

	im1 = pyfits.open('sufi_300_norm_548_004.fits')
	sufi300 = im1[0].data
	sufi300 = np.flipud(sufi300)


	im5 = pyfits.open('sufi_397_548_004.fits')
	sufi397 = im5[0].data
	sufi397 = np.flipud(sufi397)


	fig=plt.figure(figsize=(15,8),facecolor='white')
	aspect = 5
	pad_fraction = 0.5

	ax11=fig.add_subplot(1,2,1)
	im11=ax11.imshow(sufi300, extent=[0,13,0,34],cmap=pylab.gray(),vmin=(sufi300.mean()-n*sufi300.std()),vmax=(sufi300.mean()+n*sufi300.std()))

	ax11.set_xticks(np.arange(0,13,2))
	ax11.set_xticks(np.arange(0,13,0.5),minor=True)
	ax11.set_yticks(np.arange(0,34,10))
	ax11.set_yticks(np.arange(0,34,1),minor=True)
	ax11.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 18)
	ax11.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax11.set_xlabel('[arcsec]',fontsize=20)
	ax11.set_ylabel('[arcsec]',fontsize=20)
	ax11.set_title('(a)',position=(0, 1),fontsize=22)
	divider11 = make_axes_locatable(ax11)
	cax11 = divider11.append_axes("right", size=0.15, pad=0.05)
	cbar11 = plt.colorbar(im11, cax=cax11,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax11.tick_params(labelsize=16)

	ax1=fig.add_subplot(1,2,2)
	im1=ax1.imshow(sufi397, extent=[0,13,0,34],cmap=pylab.gray(),vmin=(sufi397.mean()-n*sufi397.std()),vmax=(sufi397.mean()+n*sufi397.std()))
	ax1.set_xticks(np.arange(0,13,2))
	ax1.set_xticks(np.arange(0,13,0.5),minor=True)
	ax1.set_yticks(np.arange(0,34,10))
	ax1.set_yticks(np.arange(0,34,1),minor=True)
	ax1.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 18,labelleft='off')
	ax1.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax1.set_xlabel('[arcsec]',fontsize=20)
	ax1.set_title('(b)',position=(0, 1),fontsize=22)
	divider1 = make_axes_locatable(ax1)
	cax1 = divider1.append_axes("right", size=0.15, pad=0.05)
	cbar1 = plt.colorbar(im1, cax=cax1,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax1.tick_params(labelsize=16)

	plt.subplots_adjust(wspace=-.5, hspace=None)
	plt.savefig('sufis_2013.png', dpi=300)

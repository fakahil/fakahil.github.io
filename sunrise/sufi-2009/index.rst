.. title: Visualizing the SUFI/sunrise 1 images at all wavelengths
.. slug: sufi-2009
.. date: 2020-09-25 19:30:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text



This code shows in one figure the SUFI images in all wavelengths for sunrise 1 (used for my sunrise 1 paper)

.. code-block:: python

	import pyfits
	import matplotlib.pyplot as plt
	import numpy as np
	import pylab
	from numpy import flipud as fl
	from mpl_toolkits.axes_grid1 import make_axes_locatable, axes_size

	n = 2 #sigma range


	im1 = pyfits.open('sufi_lev3.1_214_177_020.fits')
	sufi214 = im1[0].data

	sufi300 = pyfits.getdata('sufi_lev3.1_300_177_020.fits')

	im2 = pyfits.open('sufi_lev3.1_nc_313_177_020.fits')
	sufi313 = im2[0].data


	im3 = pyfits.open('sufi_lev3.1_388_177_020.fits')
	sufi388 = im3[0].data


	im5 = pyfits.open('sufi_lev3.1_397_177_020.fits')
	sufi397 = im5[0].data



	fig=plt.figure(figsize=(25,10),facecolor='white')
	aspect = 5
	pad_fraction = 0.5

	ax11=fig.add_subplot(1,5,1)
	im11=ax11.imshow(sufi214, extent=[0,13,0,38],cmap=pylab.gray(),vmin=(sufi214.mean()-n*sufi214.std()),vmax=(sufi214.mean()+n*sufi214.std()))

	ax11.set_xticks(np.arange(0,13,2))
	ax11.set_xticks(np.arange(0,13,0.5),minor=True)
	ax11.set_yticks(np.arange(0,38,10))
	ax11.set_yticks(np.arange(0,38,1),minor=True)
	ax11.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 20)
	ax11.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax11.set_xlabel('[arcsec]',fontsize=20)
	ax11.set_ylabel('[arcsec]',fontsize=20)
	ax11.set_title('(a)',position=(0, 1),fontsize=20)
	divider11 = make_axes_locatable(ax11)
	cax11 = divider11.append_axes("right", size=0.15, pad=0.05)
	cbar11 = plt.colorbar(im11, cax=cax11,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax11.tick_params(labelsize=15)

	ax1=fig.add_subplot(1,5,2)
	im1=ax1.imshow(sufi300, extent=[0,13,0,38],cmap=pylab.gray(),vmin=(sufi300.mean()-n*sufi300.std()),vmax=(sufi300.mean()+n*sufi300.std()))
	ax1.set_xticks(np.arange(0,13,2))
	ax1.set_xticks(np.arange(0,13,0.5),minor=True)
	ax1.set_yticks(np.arange(0,38,10))
	ax1.set_yticks(np.arange(0,38,1),minor=True)
	ax1.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 20,labelleft='off')
	ax1.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax1.set_xlabel('[arcsec]',fontsize=20)
	ax1.set_title('(b)',position=(0, 1),fontsize=20)
	divider1 = make_axes_locatable(ax1)
	cax1 = divider1.append_axes("right", size=0.15, pad=0.05)
	cbar1 = plt.colorbar(im1, cax=cax1,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax1.tick_params(labelsize=15)

	ax44=fig.add_subplot(1,5,3)
	im44=ax44.imshow(sufi313, extent=[0,13,0,38],cmap=pylab.gray(),vmin=(sufi313.mean()-n*sufi313.std()),vmax=(sufi313.mean()+n*sufi313.std()))
	ax44.set_xticks(np.arange(0,13,2))
	ax44.set_xticks(np.arange(0,13,0.5),minor=True)
	ax44.set_yticks(np.arange(0,38,10))
	ax44.set_yticks(np.arange(0,38,1),minor=True)
	ax44.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 20,labelleft='off')
	ax44.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax44.set_xlabel('[arcsec]',fontsize=20)
	ax44.set_title('(c)',position=(0, 1),fontsize=20)
	divider44 = make_axes_locatable(ax44)
	cax44 = divider44.append_axes("right", size=0.15, pad=0.05)
	cbar44 = plt.colorbar(im44, cax=cax44,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax44.tick_params(labelsize=15)

	ax2=fig.add_subplot(1,5,4)
	im2=ax2.imshow(sufi388, extent=[0,13,0,38],cmap=pylab.gray(),vmin=(sufi388.mean()-n*sufi388.std()),vmax=(sufi388.mean()+n*sufi388.std()))
	ax2.set_xticks(np.arange(0,13,2))
	ax2.set_xticks(np.arange(0,13,0.5),minor=True)
	ax2.set_yticks(np.arange(0,38,10))
	ax2.set_yticks(np.arange(0,38,1),minor=True)
	ax2.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 20,labelleft='off')
	ax2.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax2.set_xlabel('[arcsec]',fontsize=20)
	ax2.set_title('(d)',position=(0, 1),fontsize=20)
	divider2 = make_axes_locatable(ax2)
	cax2 = divider2.append_axes("right", size=0.15, pad=0.05)
	cbar2 = plt.colorbar(im2, cax=cax2,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax2.tick_params(labelsize=15)

	ax3=fig.add_subplot(1,5,5)
	im3=ax3.imshow(sufi397, extent=[0,13,0,38],cmap=pylab.gray(),vmin=(sufi397.mean()-n*sufi397.std()),vmax=(sufi397.mean()+n*sufi397.std()))
	ax3.set_xticks(np.arange(0,13,2))
	ax3.set_xticks(np.arange(0,13,0.5),minor=True)
	ax3.set_yticks(np.arange(0,38,10))
	ax3.set_yticks(np.arange(0,38,1),minor=True)
	ax3.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 20,labelleft='off')
	ax3.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax3.set_xlabel('[arcsec]',fontsize=20)
	ax3.set_title('(e)',position=(0, 1),fontsize=20)
	divider3 = make_axes_locatable(ax3)
	cax3 = divider3.append_axes("right", size=0.15, pad=0.05)
	cbar3 = plt.colorbar(im3, cax=cax3,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	#cbar3.set_clim(vmin=0.6,vmax=2)
	cax3.tick_params(labelsize=15)


	plt.savefig('sufis.png', dpi=500)




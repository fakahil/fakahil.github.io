.. title: Visualizing IMAX images in pairs
.. slug: imax-imax
.. date: 2020-09-16 14:33:40 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text



This code shows maps of IMAX continuum and line core intensities

.. code-block:: python

	import pyfits
	import matplotlib.pyplot as plt
	import numpy as np
	import pylab
	from numpy import flipud as fl
	from mpl_toolkits.axes_grid1 import make_axes_locatable, axes_size

	n = 2 #sigma range

	im1 = pyfits.open('cont_inv_548_001.fits')
	B = im1[0].data
	B = np.flipud(B)
	#B=np.abs(B)

	im4 = pyfits.open('LC_inv_shift_001.fits')
	cont = im4[0].data
	cont = np.flipud(cont)


	fig=plt.figure(figsize=(14,8),facecolor='white')
	aspect = 5
	pad_fraction = 0.5

	ax = fig.add_subplot(1,2,1)
	im=ax.imshow(cont, extent=[0,50,0,50],cmap=pylab.gray(),vmin=cont.min(),vmax=cont.max())#vmin=(cont.mean()-n*cont.std()),vmax=(cont.mean()+n*cont.std()))
	ax.set_xticks(np.arange(0,51,10))
	ax.set_yticks(np.arange(0,51,10))
	ax.set_xticks(np.arange(0,51,1),minor=True)
	ax.set_yticks(np.arange(0,51,1),minor=True)
	ax.tick_params(axis = 'both', which = 'major',length=6, width=2,labelsize = 14)
	ax.tick_params(axis = 'both', which = 'minor', length=4, width=2)
	ax.set_title('(a)',position=(.05, 1),fontsize=20)
	ax.set_xlabel('[arcsec]',fontsize=18)
	#ax.set_ylabel('[arcsec]',fontsize=20)
	divider = make_axes_locatable(ax)
	cax = divider.append_axes("right", size=0.15, pad=0.05)
	cbar = plt.colorbar(im, cax=cax,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cbar.set_label('IMaX line core',fontsize=20)

	cax.tick_params(labelsize=14)

	ax4=fig.add_subplot(1,2,2)
	im4=ax4.imshow(B, extent=[0,50,0,50],cmap=pylab.gray(),vmin=B.min(),vmax=B.max())#vmin=(B.mean()-n*B.std()),vmax=(B.mean()+n*B.std()))
	ax4.set_xticks(np.arange(0,51,10))
	ax4.set_yticks(np.arange(0,51,10))
	ax4.set_xticks(np.arange(0,51,1),minor=True)
	ax4.set_yticks(np.arange(0,51,1),minor=True)
	ax4.tick_params(axis = 'both', which = 'major',length=6, width=1,labelsize = 14,labelleft='off',color='white')
	ax4.tick_params(axis = 'both', which = 'minor', length=4, width=1)
	ax4.set_title('(b)',position=(0.05, 1),fontsize=20)
	ax4.set_xlabel('[arcsec]',fontsize=18)
	#ax4.set_ylabel('[arcsec]',fontsize=18)
	divider4 = make_axes_locatable(ax4)
	cax4 = divider4.append_axes("right", size=0.15, pad=0.05)
	cbar4 = plt.colorbar(im4, cax=cax4,orientation='vertical')#,ticks=np.arange(0.4,0.9,0.1))
	cax4.tick_params(labelsize=14)
	cbar4.set_label('IMaX continuum',fontsize=20)
	plt.subplots_adjust(wspace=.4, hspace=None)

	plt.savefig('cont_lc.png',dpi=300)
	plt.clf()




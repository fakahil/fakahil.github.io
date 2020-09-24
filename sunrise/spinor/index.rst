.. title: Saving the output of the SPINOR inversions on IMAX data
.. slug: spinor
.. date: 2020-09-17 12:30:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text


This code reads the atmospheric quantities returned by the SPINOR inversions (done by Tino) and saves them in separate directories


.. code-block:: python

    import numpy as np
    import matplotlib.pyplot as plt
    import pyfits
    import os
    p1  = '/home/kahil/sunrise_1/IMaX/level2.3/'
    p2 = '/home/kahil/sunrise_1/IMaX/Spinor1D_inversions_2016_Feb/'

    p_v = '/home/kahil/kahil_data/project_1/IMaX/V_los/'
    p_t1 = '/home/kahil/kahil_data/project_1/IMaX/temp_m2p5/'
    p_t2 = '/home/kahil/kahil_data/project_1/IMaX/temp_mp9/'
    p_t3 = '/home/kahil/kahil_data/project_1/IMaX/temp_p0/'
    p_B = '/home/kahil/kahil_data/project_1/IMaX/B_los_inv_abs/'
    #os.makedirs(p_v)
    #os.makedirs(p_t1)
    #os.makedirs(p_t2)
    #os.makedirs(p_t3)

    list_imax = sorted(os.listdir(p1))
    for file in list_imax:
     if file.endswith('.fits.gz') and file.startswith('imax_177_'):
         imax_ind = file[5:12]
         dir_path= p2+'Obs177_Cyc'+file[9:12]+'/lev2.2_GlobSL0.120_936x936/'
         inv_data = pyfits.getdata(dir_path+'inverted_atmos.fits')
         temp_logtm2p5 = inv_data[5,:,:]
         temp_logtm0p9 = inv_data[6,:,:]
         temp_logt0 = inv_data[7,:,:]
         F = inv_data[8,:,:]
         G = inv_data[9,:,:]
         Az = inv_data[10,:,:]
         V = inv_data[11,:,:]
         Mu = inv_data[12,:,:]
         B_LOS = np.abs(inv_data[13,:,:])
         Chi = inv_data[14,:,:]
         B_hor = F*np.sin(np.radians(G))
       
         hdu = pyfits.PrimaryHDU(V)
         hdu.writeto(p_v+'V_los_'+imax_ind+'.fits')

         hdu = pyfits.PrimaryHDU(temp_logtm2p5)
         hdu.writeto(p_t1+'temp_m2p5_'+imax_ind+'.fits')
      
         hdu = pyfits.PrimaryHDU(temp_logtm0p9)
         hdu.writeto(p_t2+'temp_mp9_'+imax_ind+'.fits')

         hdu = pyfits.PrimaryHDU(temp_logt0)
         hdu.writeto(p_t3+'temp_p0_'+imax_ind+'.fits')
         hdu = pyfits.PrimaryHDU(B_LOS)
         hdu.writeto(p_B+'B_los_abs_'+imax_ind+'.fits')

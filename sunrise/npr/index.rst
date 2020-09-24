.. title: Non parametric Regression functions applied on the scatterplots of brightness vs. Field strength
.. slug: npr
.. date: 2020-09-12 18:30:30 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text


This code contains all the python functions one could use to perform Non parametric regression on scattered data points:

.. code-block:: python


      import numpy as np
      import matplotlib.pyplot as plt
      import pyqt_fit
      import pyqt_fit.nonparam_regression as smooth
      from pyqt_fit import npr_methods
      import statsmodels
      import statsmodels.api as sm
      import pyfits
      from scipy.optimize import curve_fit
      lowess = sm.nonparametric.lowess
      from pyqt_fit import kernels

       


      im = pyfits.open('B_inv.fits')
      B = im[0].data
      x=abs(B)
      x = np.ravel(x)

      im = pyfits.open('Cont_sl0.fits')
      y = im[0].data
      y = np.ravel(y)
      #f = np.loadtxt('sub_data')
      #x = f[:,0]
      #y = f[:,1]

      #f = np.loadtxt('data')
      #x = f[:,0]
      #y = f[:,1]



      def lin_log(x,a,b):
       y = a*np.log10(x)+b
       return y

      ################################### my old way of binning
      def Binning(x,y,bandwidth):
        bins = np.arange(x.min(),x.max(),bandwidth)
        idx = np.digitize(x,bins)
        mean = [np.mean(y[idx==k]) for k in range(len(bins))]
        std = [y[idx==k].std() for k in range(len(bins))]
        f1 = np.loadtxt('bins_b_c_inv')
        a = f1[:,0]
        b = f1[:,1]
        plt.plot(a,b,label='500 pts')
        plt.plot(bins-bandwidth/2,mean,'r--',lw=4,alpha=.8,label='bandwidth')
        plt.show()

      ##########################################################
      def Method(i):

       if i==1:
         method = npr_methods.SpatialAverage()
       if i==2:
         method = npr_methods.LocalPolynomialKernel(q=1)
       if i==3:
         method = npr_methods.LocalPolynomialKernel(q=2)
       if i==4:
         method = npr_methods.LocalPolynomialKernel(q=3)
       return method  
      ########################################################## LOWESS (statsmodels)

      def LOWESS(x,y,f):
       ind = np.where((x>50) &(x<2000))
       x = x[ind]
       y = y[ind]
       #d = len(x)*0.01
       z = lowess(y,x,f)
       #k0 = smooth.NonParamRegression(x,y,method=npr_methods.SpatialAverage())
       #k0.fit()
       #k1 = smooth.NonParamRegression(x, y, method=npr_methods.LocalPolynomialKernel())
       #k1.fit()
       #k2 = smooth.NonParamRegression(x, y, method=npr_methods.LocalPolynomialKernel1D(q=2))
       #k2.fit()
       #xnew = np.linspace(x.min(),x.max(),1000)
       plt.scatter(x,y,s=0.1)
       #plt.plot(xnew,k0(xnew),c='red',lw=1.5,label='spatial averaging')
       #plt.plot(xnew,k1(xnew),c='yellow',lw=1.5,label='Local Poly(q=1)')
       #plt.plot(xnew,k2(xnew),c='gray',lw=1.5,label='Local Poly(q=2)')
       f1 = np.loadtxt('bins')
       a = f1[:,0]
       b = f1[:,1]
       ind2 = np.where(a>90)
       xnew2 = np.linspace(a[ind2].min(),2000,1000)
       popt, pcov = curve_fit(lin_log,a[ind2],b[ind2])
       ynew = lin_log(xnew2,*popt)
       plt.plot(a,b,'r',lw=2,label='binning/500 pts')
       plt.plot(xnew2,ynew,'--b',label='logarithmic fit')
       plt.plot(z[:,0],z[:,1],'g',lw=1.5,label='LOWESS')
       plt.xlabel('B_los',fontsize=16)
       plt.ylabel('Contrast',fontsize=16)
       plt.xlim(x.min(),2000)
       plt.ylim(0.6,1.4)
       plt.legend(loc='lower right')
       plt.show()



      ############################ nonparam regression using pyqt_fit

      def NonParamReg(x,y,f):
        ind = np.where((x>20)&(x<2000))
        x = x[ind]
        y = y[ind]
        z = lowess(y,x,f)
        k0 = smooth.NonParamRegression(x,y,method=npr_methods.SpatialAverage())
        k0.fit()
        k1 = smooth.NonParamRegression(x, y, method=npr_methods.LocalPolynomialKernel())
        k1.fit()
        k2 = smooth.NonParamRegression(x, y, method=npr_methods.LocalPolynomialKernel1D(q=2))
        k2.fit()
        k3 = smooth.NonParamRegression(x, y, method=npr_methods.LocalPolynomialKernel1D(q=3))
        k3.fit()
        plt.scatter(x,y,s=0.1)
        plt.xlabel('B_los',fontsize=16)
        plt.ylabel('Line core Contrast',fontsize=16)
        plt.xlim(x.min(),2000)
        plt.ylim(0.5,2.5)#(0.6,1.4)
        xnew = np.linspace(x.min(),x.max(),1000)
        f = np.loadtxt('bins')
        a = f[:,0]
        b = f[:,1]
        ind2 = np.where(a>100)
        popt, pcov = curve_fit(lin_log,a[ind2],b[ind2])
        ynew = lin_log(xnew,*popt)
        plt.plot(xnew,k0(xnew),lw=1.5,label='spatial averaging')
        plt.plot(xnew,k1(xnew),lw=1.5,label='Local Poly(q=1)')
        plt.plot(xnew,k2(xnew),lw=1.5,label='Local Poly(q=2)')
        plt.plot(xnew,ynew,lw=1.5,label='Logarithmic fit')
        #plt.plot(xnew,k3(xnew),c='gray',lw=1.5,label='Local Poly(q=3)')
        plt.plot(a,b,'--',lw=1.5,label='binning')
        plt.plot(z[:,0],z[:,1],lw=1.5,label='LOWESS')
        plt.legend(loc='lower right')
        #f = open('spatial_av','w') 
        #for i in range(k.fitted_ydata.shape[0]):
         # f.write(str(xnew[i])+' '+str(k(xnew)[i])+'\n')
        #d ddf.close()
        plt.savefig('LC_allfits')

      NonParamReg(x,y,method)
      plt.clf()
      
      #method = npr_methods.SpatialAverage()
      #method=npr_methods.LocalPolynomialKernel1D(q)




      ############### comments
      #smooth.NonParamRegression class takes as parameters the bandwidth, the covariance, the kernel function, the regression function 
      #LOWESS function is different 

      ############################################# best span=0.75

      def LOWESS2(x,y):
       ind = np.where((x>20)&(x<2000))
       x = x[ind]
       y = y[ind]
       z1 = lowess(y,x,0.1)
       z2 = lowess(y,x,0.5)
       z3 = lowess(y,x,0.8)
       xnew = np.linspace(x.min(),x.max(),1000)
       plt.scatter(x,y,s=0.1)
       plt.plot(z1[:,0],z1[:,1],'b',lw=1.5,label='LOWESS/0.1')
       plt.plot(z2[:,0],z2[:,1],'r',lw=1.5,label='LOWESS/0.5')
       plt.plot(z3[:,0],z3[:,1],'g',lw=1.5,label='LOWESS/0.8')
       f1 = np.loadtxt('bins')
       a = f1[:,0]
       b = f1[:,1]
       plt.plot(a,b,'--g',lw=2,label='binning/500 pts')
       plt.xlabel('B_los',fontsize=16)
       plt.ylabel('Contrast',fontsize=16)
       plt.xlim(x.min(),2000)
       plt.ylim(0.6,1.4)
       plt.legend(loc='lower right')
       plt.show()

      ###################################### same regression method, same kernel, different bandwidths
      Method = npr_methods.LocalPolynomialKernel1D(q=3)
      Method =npr_methods.SpatialAverage()
      def nonparamreg(x,y,h,Method):
       ind = np.where((x>30)&(x<2000))
       x = x[ind]
       y = y[ind]
       xnew = np.linspace(x.min(),x.max(),1000)
       plt.scatter(x,y,s=0.1)
       plt.title('Regression method ='+str(Method))
       plt.xlim(0,2000)
       plt.ylim(0.6,1.4)
       f1 = np.loadtxt('bins_b_c_inv')
       a = f1[:,0]
       b = f1[:,1]
       plt.plot(a,b,'--',lw=2,label='binning/500 pts')
       #z = lowess(y,x,0.75)
       #plt.plot(z[:,0],z[:,1],lw=1.5,label='LOWESS/0.75')
       k = smooth.NonParamRegression(x, y, method=Method)
       k.fit()
       plt.plot(xnew,k(xnew),lw=1.5,label='default=%.1f'%k.bandwidth+'G')
       for i in range(10,h,4):
        k = smooth.NonParamRegression(x, y, method=Method,bandwidth=i)
        k.fit()
        plt.plot(xnew,k(xnew),lw=1.5,label='bandwidth=%.1f'%k.bandwidth+'G')
       plt.legend()
       plt.show()
       
      ########################### same regression method, different kernels

      def nonparamreg(x,y,Method):
       #Method = npr_methods.LocalPolynomialKernel1D(q=2)
       ind = np.where((x>80)&(x<2000))
       x = x[ind]
       y = y[ind]
       xnew = np.linspace(x.min(),x.max(),1000)
       plt.scatter(x,y,s=0.1)
       plt.title('Regression method = Spatial Averaging')
       plt.xlim(0,2000)
       plt.ylim(0.6,1.4)
       f1 = np.loadtxt('bins')
       a = f1[:,0]
       b = f1[:,1]
       k1  = smooth.NonParamRegression(x, y, method=Method) #Gaussian
       k2 = smooth.NonParamRegression(x, y, method=Method,kernel=kernels.tricube())
       k3 = smooth.NonParamRegression(x, y, method=Method,kernel=kernels.Epanechnikov())
       k1.fit(); k2.fit(); k3.fit()
       plt.plot(xnew,k1(xnew),lw=1.5,label='Gaussian Kernel')
       plt.plot(xnew,k2(xnew),lw=1.5,label='Tricube Kernel')
       plt.plot(xnew,k3(xnew),lw=1.5,label='Epanechnikov Kernel')
       plt.legend()
       plt.show()

      ################################################ different regression methods, same kernel, same bandwidth

       #### spline smoothing

      statsmodels.quantreg
      model = QuantReg(response, X)
      fitted = model.fit(q=0.1)
      print(fitted.summary())

      #############################################################


    


  

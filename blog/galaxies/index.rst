.. title: Galaxies
.. slug: galaxies
.. date: 2020-02-12 01:52:36 UTC+01:00
.. tags: galaxies, astronomy
.. category: 
.. link: 
.. description: 
.. type: text

Here is a picture of a galaxy

.. image:: /images/galaxy.jpg


.. code-block:: python
   :number-lines:

   def sieve_of_eratosthenes():
       factors = defaultdict(set)
       for n in count(2):
           if factors[n]:
               for m in factors.pop(n):
                   factors[n+m].add(m)
           else:
               factors[n*n].add(n)
               yield n

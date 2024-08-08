----------------------------------------------

Halo catalogs for TNG

----------------------------------------------


HOW TO READ THIS DATA:

	The HDF5 files were written using the pandas python package.
	You can quickly load them and turn them into 2D numpy arrays
	using the following commands:

	*****

	import pandas as pd

	Halos = pd.read_hdf('PATH TO HDF5 FILE', key = 'Halos').to_numpy()
	

	*****

	Halos will then be a 2D numpy array that you can index into using slicing. 

	If you leave them as pandas dataframes you can also quickly access
	an individual column and turn it into a numpy array via:

	******

	Pandas_Series = Halos[quantity_name]
	Numpy_array   = Halos[quantity_name].values

	
	******

FILE NAMING:
	Each file starts with the sim run (TNG300-1) and then follows with the snapshot.

---------------------------------------
---------------------------------------

Halo Catalog properties

	We select all halos with M500c > 1e13 Msun at z = 0
	For higher redshifts, we track the z=0 halos back in time
	and compute profiles only for their progenitors. This is
	done by tracking the halo's central subhalo back in time via
	the SUBLINK subhalo merger tree.

	Profiles are computed similar to how ROCKSTAR computed DM profiles
	for computing cvir. Profiles are then interpolated with Cubic Splines
	so they all share a common radial grid. Profiles go from [0.03, 2.0]*R500c


!!!! NOTE: The profile radii (in units of R500c) can be obtained as R = np.geomspace(0.03, 2, 25) !!!!

---------------------------------------	
---------------------------------------

HaloID_LastDesc:
	The HaloID of the z = 0 halo (i.e. the last descendant) associated with this object.


SubhaloID_LastDesc:
	Same as above but for the Subhalos. Note that the Subhalos are how we track the
	halos back in time.


HaloID, SubhaloID:
	The Halo and SubhaloID of this subhalo at a given snapshot.
	

M500c_TNG, M200c_TNG:
	The mass of halo (defined as mass within sphere with a specific enclosed density). 
	The precomputed quantity from TNG. In units log10(msun).


R500c_TNG, R200c_TNG:
	The associate radius of the sphere mentioned above. In units of physical kpc.


MinRadius:
	The force softening scale in units of R500c, defined as MinRadius = epsilon/R500c.
	The profile below this scale is normally not measured.


Ndm, Ngas, Nstars, Nbhs:
	The total number of particles (per type of DM, Gas, Stars, BHs) used to construct profiles.
	

rhoDM_bin{0..24}:
	The DM mass density in 25 radial bins. In units of Msun/kpc^3 (physical units, not comoving).


rhoStar_bin{0..24}:
	Same as above but for stellar density.


rhoGas_bin{0..24}:
	Same as above but for gas density.


PGas_bin{0..24}:
	The electron pressure, computed as P = N_e(r) * T(r) / V(r), in radial shells. Units of K/cm^3


TGas_bin{0..24}:
	The mass-weighted gas temperature computed in radial shells. Units of K.


Mbh_Cen:
	The mass of the SMBH at the center of the central subhalo (i.e. the SMBH in the cluster core).
	Also includes the gas reservoir around the SMBH. In units of Msun.


Mbh_clean_Cen:
	Same as above but without including the gas reservoir component.


Endstate:
	A string variable that logs where the pipeline ended for a given halo.
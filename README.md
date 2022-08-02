# CMB-Inpainting-
First of all we need to install healpy package.

```
! pip install -q 'healpy==1.13.0' 'astropy==4.0' 
```

```
cl = np.load('./cl_planck_lensed.npy') 
ll = cl[ : ,0]
cl = cl[ : ,1]

arrayp=hp.sphtfunc.synfast(cl,2048)  


ll=ll[:3000]
cl=cl[:3000]
plt.figure(figsize=(10, 8))
plt.plot(ll, ll * (ll + 1) * cl/2*np.pi )
plt.xlabel("$\ell$")
plt.ylabel("$\ell(\ell+1)C_{\ell}$")
plt.grid()
```
![download (3)](https://user-images.githubusercontent.com/35627868/182391905-5e4956a2-947b-423b-a0f4-fb9e9836ed25.png)
which is Power spectrum of CMB (plank's data) 

```
hp.mollview(
    arrayp,
    coord=["G", "E"],
    title="Histogram equalized Ecliptic",
    unit="mK",
    norm="hist",
    cmap='jet'
)
hp.graticule()

```
![download (4)](https://user-images.githubusercontent.com/35627868/182392552-72938908-b0cd-4e9a-8408-0c3e245e5b6c.png)
 
 then by using COM Mask for CMB I had masked the map.
 ```
 planck_mask = hp.read_map('COM_Mask_CMB-common-Mask-Int_2048_R3.00.fits').astype(np.bool_)
 
wmap_map_I_masked = hp.ma(arrayp)
wmap_map_I_masked.mask = np.logical_not(planck_mask) 


hp.mollview(wmap_map_I_masked.filled(),cmap='jet')
 ```
 ![download (5)](https://user-images.githubusercontent.com/35627868/182393360-038af9ff-c9b5-4573-a6ff-270a8c2de408.png)

Then for proceeding the work we need to install [ccg pack] (https://github.com/vafaei-ar/ccgpack) 

```
!pip install git+https://github.com/vafaei-ar/ccgpack.git

from ccgpack import sky2patch,ch_mkdir,pop_percent,download
```
I use this package to divid the map into smaller patches:
```
patch = sky2patch(wmap_map_I_masked ,npatch=1)

fig,ax=plt.subplots(3,4,figsize=[9,6], sharex=True, sharey=True)
ax.shape
for j in range(4):
  for i in range(3):
    ax[i,j].imshow(patch[i+j])
```


 ![download (6)](https://user-images.githubusercontent.com/35627868/182395144-b71f20c6-0a4a-4894-ba3e-797caaed1b37.png)

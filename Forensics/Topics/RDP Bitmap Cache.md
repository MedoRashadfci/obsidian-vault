The RDP Bitmap Cache is a forensic artifact that’s rarely spoken of, but can yield some quick wins in an investigation.

According to the official Microsoft documentation: “_Bitmap caches are used by the client and server to store graphic bitmaps. Each bitmap cache holds bitmaps of a specified size in pixels (known as the “tile size”). If a bitmap does not fit into a single cache entry, the server uses a tiling algorithm to divide the bitmap into tiles that will fit into the cache entries so that they can be stored separately into the cache_.”

**Where to find the cache and parsing it** Starting from Windows 7 and up you’ll find the RDP Bitmap Cache in the following location:

`C:\\Users\\<USER>\\AppData\\Local\\Microsoft\\Terminal Server Client\\Cache\\`

![[Pasted image 20260507031702.png]]


and **Cache0000.bin**

**BMC Tools**

Enter [BMC Tools](https://github.com/ANSSI-FR/bmc-tools) which is a wonderful open-source tool that allows you to parse the RDP Bitmap Cache into something usable for your investigation. BMC Tools are written in Python and can easily be run on both Linux or Windows via the Command line like so:

`bmc-tools.py -s cache0000 -d cache0000_parsed`

![[Pasted image 20260507031857.png]]

and now we have a directory called **extracted** that have the content of the cache.

**RDP Cache Stitcher**
[**RdpCacheStitcher**](https://github.com/BSI-Bund/RdpCacheStitcher) is a tool that supports forensic analysts in reconstructing useful images out of RDP cache bitmaps.

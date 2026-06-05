
Enter [BMC Tools](https://github.com/ANSSI-FR/bmc-tools) which is a wonderful open-source tool that allows you to parse the RDP Bitmap Cache into something usable for your investigation. BMC Tools are written in Python and can easily be run on both Linux or Windows via the Command line like so:

`bmc-tools.py -s cache0000 -d cache0000_parsed`

![[Pasted image 20260507031857.png]]

and now we have a directory called **extracted** that have the content of the cache.
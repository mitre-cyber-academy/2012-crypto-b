Crypto Challenge, 300 points:

Summary: Flag appears as text in an image, which is then encrypted using 
Blowfish in a weak mode of operation.  The encrypted image and the 
encryption algorithm is available; the image encrypted was in .bmp format 
which is hinted at in the file name.  The image is encrypted in ECB mode, 
which is not secure.  I think this challenge is made a lot easier by the
fact that a google search for "encrypted bitmap wikipedia" returns a 
page "Block cipher modes of operation" as its second result which gives an
example of exactly the trick they need to use.

Challenge files:

	index.html -- The challenge entry point.
	/files -- this should be a readable sub-folder.
      /files/flag.bmp.encrypted -- pointed to by index.html

Solution walkthrough:

	The problem with the encryption method used to encrypt flag.bmp is 
that it uses ECB, an insecure mode of operation.  

	flag.bmp is the actual file that was encrypted.  This is just for 
reference, students should not be able to easily recover it.

      From flag.bmp.encrypted, students have to work to understand the 
.bmp file format.  They'll need to make some guesses; I would expect it's 
likely students will try using Windows paint to export into .bmp file, 
which (with the 24-bit color setting) produces the right kind of headers.  
They'll have to figure out what the size is; from the filesize of 
flag.bmp.encrypted, given the correct color settings and the 64-bit IV, 
the file would contain 1024000 2/3 pixels.  Naturally this suggests 1024000 
pixels with 2 bytes of padding.  There are a number of likely image sizes 
(1024x1000 springs to mind, but it turns out to be 1280x800).  A .bmp file 
header can be created by hand; Wikipedia has a good explanation of the 
format with an example.  Students will find that overwriting the first N 
bytes with a basic .bmp header doesn't work, because the file was padded 
by 2 bytes; this can be learned from the algorithm and observing the file 
sizes of flag_original.bmp and flag.bmp.encrypted.  

      Walkthrough continues: open both flag.bmp and flag.bmp.encrypted
in a hex editor such as the one included.  Copy the first 54 bytes from 
flag.bmp to flag.bmp.encrypted.ecb.  Delete the last byte also, to make the 
pixel array the correct size.  Save this as a .bmp file.  flag.bmp.ecb.bmp 
is the result of this process.

	Open flag.bmp.ecb.bmp in an image viewer.  The image is garbled but 
the flag string pops out clearly.

PREV [[Computer Vision/Introduction]]


![[Pasted image 20240115100557.png]]

Lighting:
	![[Pasted image 20240115100804.png]]
	
	![[Pasted image 20240115100816.png]]
	This is a general model to calculate light emitted
	Diffuse Reflection
		![[Pasted image 20240115101121.png]]
	We must account for wavelength due to colour of the surface
	Specular reflection
		![[Pasted image 20240115101245.png]]
		The reflected light have same angle, all energy reflected to input == output
	Ambient Illumination 
		accounts for general illumination that is complicated to model: many light sources, distant sources, reflection on many walls etc.
		![[Pasted image 20240115101514.png]]
	Combining all of these can lead to photorealistic graphics.

Optics:
	![[Pasted image 20240115101639.png]]
	Human Optics:
		![[Pasted image 20240115101804.png]]
		Cone cells use trichromatic vision, so computers model this via RGB. 
		![[Pasted image 20240115101948.png]]
		Rod cells: More sensitive to light.
	Camera Sensors:
		Two types of sensors:
			CCD, CMOS (more popular on phones)
		Sensors convert photons to electron charges and record the values
		![[Pasted image 20240115102233.png]]
		![[Pasted image 20240115102408.png]]
		Interpolation using neighbour pixels is used to get (R,G,B) values from bayer filter
		![[Pasted image 20240115102515.png]]
	Representation in Computers:
		![[Pasted image 20240115102609.png]]
		Each channel is typically either:
			8 bits: 0 to 255 (most common)
			16 bits: raw camera files
	Quantisation:
		![[Pasted image 20240115102934.png]]
Graphics vs Vision
	Vision is Image -> Object; Graphics is Object -> Image; somewhat inverse
	Training vision may involve label maps eg:
		![[Pasted image 20240115103315.png]]
	Photorealistic games can be used to synthesise training data with existing semantic annotations, avoiding costs for labelling and categorising datasets etc.

NEXT [[Image Filtering]]
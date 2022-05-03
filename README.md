# Astrometry from CCD Images

Abstract

Astrometry is one of the most useful tools to astronomers as it allows for measurements of motion, distance, and much more. This report uses astrometry to calculate the aberrations in the lenses on the Nickel 1m Telescope at Lick Observatory by comparing centroids of measured stars with that of the vizier catalog. We also use astrometry to calculate the positions of the asteroids Metis and Polyhymnia. The positions are calculated over multiple hours so that the proper motion can be derived. The total proper motion of Metis was found to be 23.80 arcseconds per hour while Polyhymnia was much slower with a proper motion of 2.274 arcseconds per hour. While the error was not properly counted, the residuals of the measurements average about 10^(-4) degrees or about 0.4 arcseconds which is very precise. The measurements of the coordinates also seemed to agree with their actual predicted values of the asteroids.


1. Introduction

	Many celestial objects are so far away that they hardly seem to move at all from our reference. There are, however, much closer stars and objects in our solar system that move across the celestial sphere. Finding the motion of these objects is very important in astronomy. Knowing the motion of an object leads to information about that object such as its mass and how it is affected by the gravity of nearby objects. In the case of asteroids in the asteroid belt, which is what we’ll be observing, the motion can tell us information about how the solar system was formed. While we can measure the radial velocity (the speed that is going directly towards or away relative to earth) of an object by using the doppler shift on the object’s elemental spectrum, we must use astrometry to find out the proper motion (how it moves across the sky). Astrometry uses a reference frame of stars that we know the coordinates of and are also considered motionless. If an object changes position relative to these stars, we can calculate its right ascension and declination at each point to find its proper motion. This is exactly what we will be doing in this lab. We will observe two different asteroids, Polyhymnia and Metis, over multiple hours to calculate their proper motion across the sky. This lab also focuses on accounting for the aberrations of the lenses. If we consider these distortions constant, we can use the method of linear least squares to find the plate constants of the telescope to properly translate from pixel coordinates to celestial coordinates.

2. Observations and Data

	Data was gathered using a remote access lab that controlled the Nickel 1-Meter Telescope at Lick Observatory near San Jose. The usual bias and flat frames were taken. The bias frames were taken by doing a 0 second closed shutter exposure. The flat frames were exposures of an evenly illuminated part of the observatory at varying times and using various filters. Most of the images taken of the asteroids are in the red filter. These images were also varied in exposure time (anywhere from 3 to 60 seconds). The reason for this is that in order to effectively use astrometry to calculate the RA (Right Ascension) and Dec (Declination), we want as many background stars as possible. However, if we take too long of an exposure, then the brighter stars and the asteroid itself saturate and can no longer be used to calculate their pixel positions. For this reason, deep field (long exposures) and shallow field frames (short exposures) were taken for each measurement of an asteroid (Figure 1). 

![Image](https://imgur.com/iV5PYFO)
Figure 1: Example images of Metis: Left, 3 second exposure. Right, 30 second exposure

	The asteroids observed were picked based on their visibility the night of the observation. Objects that have an absolute hour angle greater than 3 hours generally have too much atmosphere between it and us. The closer to the zenith an object is, the more clearly we can observe it without a large atmospheric column density getting in the way. This in mind, Metis and Polyhymnia were the best candidates for the night. 

3. Data Reduction & Methods

As always, before using our data, we must reduce the images down for more accurate measurements. First we want to take the average per pixel of the bias frames so we get the most general bias to subtract from both our science images and our flat images. Next we categorize our flats by filter, since different filters have different effects on the image (Figure 2). Each flat can then be normalized by dividing by the median value of the whole image. If the CCD (Charge-Coupled Device) doesn’t have any broken pixels, then the science frame we are using can be bias subtracted and divided by the normalized flat. However, our CCD has broken pixels that will be zero after bias subtracting as shown in Figure 2. Dividing by this would cause an error, so we ignore the entire column by setting it equal to the median of the flat image. Once this is done, we have a fully reduced science frame. 
 
Figure 2: Flat image taken with the red filter showing the broken pixels and dust on the screen

4. Calculations & Modeling

	With our reduced data, we need a way to quickly calculate the pixel positions of the stars in our image for reference. Doing this in one dimension (as with a spectrograph) is simple since we can easily see where there are peaks on the graph, the peaks indicating stars. Peaks in two dimensions can be handled similarly. As shown in Figure 3, there are peaks that we can break it down into the x and y coordinates. What we can then do is go through each row and column and find the peaks. The coordinates or the rows and columns that match are our star peaks (see Appendix 1). We also double check if there are a minimum number of “bright” pixels around the peak. Bright pixels being defined as some multiple of the median value of the frame, usually twice the median. 
 
Figure 3: Left, 3D representation of the star peaks in 2D. Top-Right and Bottom-Right, slices taken from particular x and y coordinates.

	Once we have the peaks in terms of integer pixels, we want to find the actual centroid location of the star since it will not be perfectly centered on a pixel. We do this by using Equation 1. We can also calculate the error in the centroid using Equation 2.

⟨p⟩=(∑▒〖p_i I_i 〗)/(∑▒I_i )                                                                       (1)

σ_⟨p⟩ =(∑▒〖I_i (x_i≺x>)^2 〗)/(∑▒I_i )^2                                                           (2)

where, p∈x,y is the position coordinate and I_p is the intensity at position p. i are the values over a window of fixed size. The size the window should be is determined by eye but is usually about 20 pixels in diameter. An example summary of values is shown in Table 1.

 
Table 1: Summary of calculated peaks centroids, and error in centroids for the stars around the first images of Polyhymnia

	To translate these centroids to celestial coordinates, we first need to match them with the proper stars. To do this, we use vizier to get a catalog of stars near our RA and Dec, specifying the limiting magnitude and field of view. What we get is shown in Figure 4. It’s a bit of a mess, but if we translate the RA and Dec of the cataloged stars to standard coordinates (radians in spherical coordinates around the unit sphere) and translate that to pixels, we can get a better idea of which stars go with which. This translation is shown in equations 3 and 4. This is for reference only, however, since we will translate from standard to celestial assuming that the lenses in the telescope are ideal.
                                        (3)

Where X and Y are in standard coordinates, α and δ are RA and Dec, and α_0, δ_0 are the RA and Dec of the center of the catalog frame.
                                                         (4)

Where x and y are the pixel locations, f is the focal length (16,840mm for the Nickel Telescope), p is the pixel width (0.015mm for the CCD we are using which in our case is being binned so multiplied by 2 = 0.030mm), and x_0 y_0 are the translation constants. We fiddle with the translation constants to get a good match between the centroids and the stars as shown on the right in Figure 4. With a close enough match, the process for matching which numbers go to which is done by calculated the distance from the centroids to all the stars. The centroid is matched to the cataloged star that is the closest or is removed if there isn’t one close enough. 

  
Figure 4: Left, initial coordinates of stars where pixels and celestial coordinates are on the same plane. Right, locations of stars and centroids after the stars have been translated to pixel locations assuming an ideal lens. This is again taken from the early Polyhymnia images as an example.

	Knowing the true RA and Dec of our pictured stars allows us to calculate the aberrations and distortion in our lenses. We calculate the so-called plate constants that will help translate from on coordinate system to another using a linear least squares method. Assuming the plate constants are in fact constant, then all types of distortion (Translation, shear, rotation, and magnification) can be accounted for and solved for in the matrix in Equation 5.

                                           (5)
Which can be plugged into Equation 6

x=TX                                                                           (6)

where x is the vector (x,y,1) and X is (X,Y,1). Since we know f , p, and the equations of conditions that make up x and X, we can use the general least squares formula as shown in equation (7)
c=(B^T B)^(-1) B^T a                                                                (7)

where 
 

and similarly for y. Once we solve the equations for x and y, then we can simply multiply both sides of Equation 6 by  T^(-1). To minimize error, we’ll calculate the plate constants for the different periods the asteroids are observed. An example getting from peaks to celestial coordinates is shown in Figure 5. The residuals from the Figure 5 measurements are shown in Figure 6.

  
Figure 5: Left, image of Metis asteroid with surrounding stars. Right, scatter plot of both the actual star coordinates and the translated coordinates from the measured centroids, along with the measured asteroid location. 

 
Figure 6: Residuals of cataloged stars and measured stars.

Most of the measured stars have an error under 1 arcsecond except for that star at the end. That star is the two bright stars closest to the center of the Figure 5 left. Because these two stars are cataloged as one star, the calculated coordinates must have ended up somewhere between the two stars. This means that no matter what we do or which of the two we choose, the measured RA and Dec will always be farther off from the cataloged value, so we’ll scrap that data point for now. 
	Each asteroid was recorded at different times, Polyhymnia was observed at four different times and Metis three. Following the above procedure for all these asteroids, we can plot their positions as they move across our field of view as shown in Figure 7.
  
Figure 7: Left, Metis multiple measured positions with background stars. Right, Polyhymnia which moved so slowly it is a zoomed in view where no stars are pictured.

In the Polyhymnia image, one of the asteroid positions is far off from the line that the other three are forming, so we will remove that data point when calculating the proper motion. 
	Now we can calculate the proper motion of each asteroid. Instead of using a linear least squares fit, there are only three points for each so we will use the slope between the two far points. The motion on each axis of each asteroid is shown in Figure 8.

 
 
Figure 8: Top, Metis RA and Dec vs time. Bottom, Polyhymnia RA and Dec vs time.

From these slopes we can calculate the total proper motion of each asteroid. Shown in Table 2 is the summary of the results.





	Total proper motion (arcs/hr)	Direction	RA proper motion (arcs/hr)	RMS Ra
(deg)	Dec proper
Motion (arcs/hr)	RMS Dec
(deg)
Metis	23.80
	West	-23.71

	0.00013	2.159
	0.0000909

Polyhymnia	2.274
	Up and West	-1.143

	0.000119	1.966
	0.00005623


Table 2: Summary of motion of asteroids where the RMS of the residuals


5. Discussion

In this lab we showed how effective astrometry is. From background stars you can identify the distortions in your own telescope as well as calculate the positions of asteroids. Most of the data seemed to be precise. However, there still was one asteroid measurement that seemed to be far off. During the second periodic measurement of Polyhymnia, there must have been a shift in the direction of the telescope. The residuals from the calculated stars and the catalog stars were still very small, so it implies that there must have been a systematic error to get the shift we see in Figure 7. The RMS of the residuals was calculated as opposed to actual error which means that we don’t have a proper estimate of the error, but because the residuals are so small it implies that the data taken was precise. The proper motion of Metis was 23.80 arcseconds per hour, which might be fast, but to show how slowly that moves across the sky, the moon moves at a rate of about 1800 arcseconds per hour. If we observed the asteroids more frequently and calculated the magnitude at each point, we could also predict it’s period of rotation and possibly what shape they are, neither of which are spherical since they are so small (Metis is about 100km and Polyhymnia is about 50km in diameter)

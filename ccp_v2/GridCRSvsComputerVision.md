Grid CRS vs. Computer Vision
While the equal area grids are good for statistical purposes, when utilizing computer vision, the distortion of the shapes due to the projection could hinder the computer vision work.

Thought-in-progress
Concluding this though in progress :-) I think it is good to consider that a non-conformal projection distort shapes. When used in a limited domain it might still be locally conformal and then it is a moot point. Otherwise, in case of computer vision DL, I would more focus on training on better generalisation, make sure that possible distortions are represented in the training data, and/or include data augmentations (e.g. skew) that mimic such distortions.
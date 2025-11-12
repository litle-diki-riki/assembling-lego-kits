Report: LEGO Kit Detection and Fault Analysis System



Exercise 3: Kit Image Analysis with Rotation Handling

3a) Brick Detection and Centroids



Approach:

* Implemented rotation-aware brick detection using cv.minAreaRect() to handle bricks at arbitrary angles
* Each brick's rotated bounding box, centroid, and orientation angle are computed
* Color detection performed in HSV color space with optimized ranges for each color (Red, Yellow, Green, Blue)
* White brick detection was made optional (disabled by default to avoid false positives)



Key Features:

* Rotation handling: Uses minimum area rectangles instead of axis-aligned bounding boxes
* Centroid calculation: Computed using image moments for accurate center positioning
* Size classification: Compares detected dimensions (in mm) against Table 1 specifications with 10.5mm tolerance
* Visual output: Green bounding boxes with red centroids and labeled with color and size



Results:

All 6 kit images were successfully processed, detecting 7 bricks per image with their centroids, rotation angles, and dimensions accurately identified.





3b) Brick Inventory Table



Approach:

* Created structured inventory for each image counting bricks by size and color
* Results stored in pandas DataFrame for easy manipulation
* Output formatted as a clean table showing Image → Size → Color → Quantity



Results:

Successfully generated complete inventory tables for all images (ImageA, ImageB, ImageC, kit1, kit2, kit3).





3c) Kit Matching



Approach:

* Created unique "signatures" for each kit based on their brick composition
* Signature comparison: converts each inventory to a sorted string representation
* Exact matching algorithm: compares test images against reference kits



Results:

ImageA\_kit.png  -> kit1.png (100% match)

ImageB\_kit.png  -> kit3.png (100% match)  

ImageC\_kit.png  -> kit2.png (100% match)



All three test images were correctly matched to their corresponding reference kits.





3d) Recommendations for Improvement



Four key recommendations were provided:

* Improved Lighting: Consistent, controlled lighting to eliminate shadows
* Enhanced Calibration: Automated calibration checks with real-time parameter adjustment
* Multi-angle Imaging: Multiple cameras for 3D information and better occlusion handling
* Optimized Background: Matte, neutral-colored background to improve segmentation





Exercise 4: Fault Detection System



Approach

Reference Kit Loading:

* Analyzed three reference kits (Kit A, B, C) to establish baseline specifications
* Each kit contains 6 bricks with specific color-size combinations



Fault Detection Algorithm:

1. Detect all bricks in test image
2. Compare against all three reference kits
3. Calculate match percentage based on exact brick matches
4. Identify best matching kit
5. Generate detailed fault report showing:

&nbsp;	MISSING: Expected bricks not found

&nbsp;	EXTRA: More bricks than expected

&nbsp;	UNEXPECTED: Bricks not in kit specification







Results Summary

Image			Matched Kit	Score		Status		Issues

ImageA\_fault.png	Kit B		83.3%❌ 	FAULTY		1 issue

ImageB\_fault.png	Kit A		100.0%✅ 	COMPLETE 	0 issues

ImageC\_fault.png	Kit C		83.3%❌ 	FAULTY		2 issues

ImageE\_fault.png	Kit B		100.0%✅ 	COMPLETE	0 issues

ImageF\_fault.png	Kit A		83.3%❌ 	FAULTY		2 issues

ImageD\_fault.png	Kit C		100.0%✅ 	COMPLETE	0 issues



Detected Faults

ImageA\_fault.png (Kit B):

&nbsp;	Blue R2x2 is missing



ImageC\_fault.png (Kit C):

&nbsp;	Blue 2x2 should not be in kit (found 2)

&nbsp;	Blue 2x6 is missing



ImageF\_fault.png (Kit A):

&nbsp;	Yellow 2x1 is missing

&nbsp;	Blue 2x1 should not be in kit (found 1)



System Performance

* Success Rate: 100% correct kit identification (all 6 images matched to correct kits)
* Fault Detection: 3 complete kits verified, 3 faulty kits identified with specific fault descriptions
* Accuracy: System correctly identifies missing, extra, and unexpected bricks





Technical Implementation Highlights

1. Camera Calibration: Used best calibration (calib2) with 0.0074 pixel reprojection error
2. Pixel-to-MM Conversion: 0.3298 mm/pixel for accurate size measurements
3. Color Range Optimization:

&nbsp;	Yellow: Higher saturation threshold (120) to reduce false positives

&nbsp;	Blue: Adjusted range (85-135 Hue, 80+ Sat/Val) for better detection

4\. Rotation Tolerance: 10.5mm tolerance for size classification to handle measurement variations

5\. Robust Matching: Percentage-based scoring system with detailed difference tracking



The system demonstrates robust performance in detecting, classifying, and verifying LEGO brick kits with automatic fault identification capabilities suitable for production line quality control.


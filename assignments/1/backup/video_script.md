# Lane Detection — Video Presentation Script

**Duration**: 4 minutes max | **Team**: 5 members
**Format**: Each member speaks for ~45–50 seconds

---

## Member 1: Introduction & Problem Statement (~45 sec)

> "Hi, we are [Team Name / Team Number] and today we'll present our solution for Problem 2 — Lane Detection using Classical Computer Vision and Machine Learning.
>
> The goal is to detect straight lane lines on road images using a pipeline that combines image processing with ML-based line fitting. We are NOT using deep learning — this is entirely classical CV.
>
> Our pipeline has five stages: preprocessing, Canny edge detection, region-of-interest masking, Hough transform for line detection, and ML-based fitting using three different methods.
>
> We tested on 6 diverse images — a night highway, daylight roads with buildings and trees, multi-lane highways with traffic, and a challenging scene with a large tow truck. Let me hand over to [Member 2] who'll walk through the preprocessing."

---

## Member 2: Preprocessing & Edge Detection (~45 sec)

> "For preprocessing, we first convert the image to grayscale and apply CLAHE — Contrast Limited Adaptive Histogram Equalization. This is critical for night images like image0, where CLAHE boosts the contrast between headlight-illuminated lane markings and the dark road.
>
> Next, we convert to HLS colour space and create two masks: a white mask with lightness threshold of 160 to capture white lane markings, and a yellow mask with hue range 10–35 to capture yellow centre lines. We blend these masks with the grayscale at 100% gray plus 50% enhanced, then apply Gaussian blur with a 5×5 kernel.
>
> For edge detection, we use the Canny algorithm with thresholds 50 and 150, following the standard 1:3 ratio. As you can see in the notebook, the Canny output clearly picks up lane edges across all lighting conditions. [Member 3] will explain the ROI and Hough transform."

---

## Member 3: ROI, Hough Transform & Slope Classification (~50 sec)

> "We define a trapezoidal Region of Interest to focus on the road ahead and exclude the sky and surroundings. The ROI adapts to the image aspect ratio — landscape images use a narrow trapezoid at 60% height, while wider images get a broader trapezoid.
>
> We then apply the Probabilistic Hough Transform with rho=1 pixel, theta=1 degree, a threshold of 30 votes, minimum line length of 20 pixels, and maximum gap of 200 pixels — that large gap helps bridge dashed lane lines.
>
> Each detected segment is classified by its slope: negative slopes below minus 0.3 go to the left lane group, positive slopes above 0.3 go to the right lane group, and anything in between is discarded as horizontal noise. For our best images, this gives a clean 50-50 split — for example, image2 had 5 left and 5 right with zero discarded. Now [Member 4] will cover the ML fitting methods."

---

## Member 4: ML-Based Line Fitting & Method Comparison (~50 sec)

> "We implemented three ML methods to fit representative lane lines from the classified segments.
>
> **Simple Averaging** computes the mean slope and intercept — fast and deterministic but sensitive to outliers.
>
> **RANSAC** iteratively samples subsets, fits a model, and keeps the one with the most inliers. This makes it robust to outlier segments from shadows or vehicle edges.
>
> **K-Means with k=1** computes the centroid in slope-intercept space, which is mathematically equivalent to averaging for k=1 but extends to multi-lane detection with higher k values.
>
> Our results show that for clean images like image2 and image3, all three methods produce identical results. But for the cluttered image5, RANSAC gives a different slope — minus 0.45 versus minus 0.40 — showing its outlier rejection in action. Overall, RANSAC is the most robust method. [Member 5] will present the results and limitations."

---

## Member 5: Results, Limitations & Conclusion (~50 sec)

> "Looking at our results across 6 images: image0, image2, and image3 show good detection with both lanes converging at the vanishing point. Image1 and image4 show partial detection — one lane is well-detected but the other has very few segments due to faint markings.
>
> Image5 is our failure case — a highway scene with a large tow truck. It generated 212 Hough segments, 96% of which were correctly discarded as noise. But the surviving 9 segments came from the truck body and highway barrier, not the actual lanes, so the fitted lines form an X-pattern instead of converging.
>
> This highlights the key limitation: the classical Hough pipeline relies purely on edge geometry and has no semantic understanding. It cannot distinguish a lane line from a barrier edge if they share similar slopes. For such cluttered scenes, deep learning approaches like LaneNet would be needed.
>
> In conclusion, our pipeline works well for clean roads with visible markings and demonstrates three ML fitting approaches with clear mathematical justification. Thank you."

---

## Tips for Recording

- **Screen share** the notebook and scroll through relevant cells as each person speaks
- **Key visuals to show** while speaking:
  - Member 1: Dataset overview grid (all 6 images)
  - Member 2: Preprocessing pipeline figure (show for 2–3 images), Canny edge grid
  - Member 3: ROI boundary + masked edges, Hough segments grid
  - Member 4: ML fitting results table, method comparison side-by-side (3 methods on one image)
  - Member 5: Pipeline results grid (all 6 images), metrics bar charts, analysis markdown
- Keep transitions quick — "I'll hand over to [name]" is enough
- Practice once to ensure you stay under 4 minutes

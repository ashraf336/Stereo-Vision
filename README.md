# Stereo-Vision
Contributors:

Ahmed Ashraf 

Abdulrahman Habib

Mohamed Aiman 

Osama  Sherif

1 Stereo Vision
In this part  we will implement and test some simple stereo algorithms.
In each case we will take two images Il and Ir (a left and a right image) and compute the
horizontal disparity (ie., shift) of pixels along each scanline. This is the so-called baseline stereo
case, where the images are taken with a forward-facing camera, and the translation between
cameras is along the horizontal axis.

1.1 Block Matching
To get the disparity value at each point in the left image, we will search over a range disparities,
and compare the windows using two different metrics: Sum of Absolute Differences (SAD) and
Sum of Squared Differences. Do this for windows of size w where w = 1, 5 and 9. The disparity,
d.

1.2 Dynamic programming
Consider two scanlines Il(i) and Ir(j). Pixels in each scanline may be matched, or skipped
(considered to be occluded in either the left or right image). Let dij be the cost associated with
matching pixel Il(i) with pixel Ir(j). Here we consider a squared error measure between pixels
given by:
dij =
(Il(i) − Ir(j))2
σ

2
where σ is some measure of pixel noise. The cost of skipping a pixel (in either scanline)
is given by a constant c0 . For the experiments here we will use σ = 2 and c0 = 1. Given
these costs, we can compute the optimal (minimal cost) alignment of two scanlines recursively
as follows:
1. D(1, 1) = d11
2. D(i, j) = min(D(i1, j1) + dij , D(i1, j) + c0, D(i, j1) + c0)
The intermediate values are stored in an N-by-N matrix, D. The total cost of matching two
scanlines is D(N, N ). Note that this assumes the lines are matched at both ends (and hence
have zero disparity there). This is a reasonable approximation provided the images are large
relative to the disparity shift. Given D we find the optimal alignment by backtracking. In
particular, starting at (i, j) = (N, N), we choose the minimum value of D from (i - 1, j - 1),
(i - 1, j), (i, j - 1). Selecting (i - 1, j) corresponds to skipping a pixel in Il (a unit increase
in disparity), while selecting (i, j - 1) corresponds to skipping a pixel in Ir (a unit decrease in
disparity). Selecting (i - 1, j - 1) matches pixels (i, j), and therefore leaves disparity unchanged.
Beginning with zero disparity, we can work backwards from (N, N ), tallying the disparity until
we reach (1, 1).

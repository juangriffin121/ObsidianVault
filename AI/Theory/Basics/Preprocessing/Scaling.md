### Normalization

Scale and translate the inputs so that the minimal value is 0 and the maximum is 1

[[Functions#^32b04d]]
### Standardization

^d3ad36

Substract the mean value and divide by the standard deviation. (how many standard deviations away from the mean is this input).
Standardization does not restrict values to a specific range. However, standardization is much less affected by outliers. For example, suppose a district has a median income equal to 100 (by mistake), instead of the usual 0–15. Min-max scaling to the 0–1 range would map this outlier down to 1 and it would crush all the other values down to 0–0.15, whereas standardization would not be much affected. 


![[Pasted image 20231127232502.png]]
Scaling inputs when training with [[Gradient Descent]] is really useful since if the data has different scales, it will make the parameters associated have different gradient scales, and this can make diminish gradients.
This procedure can be done in deep layers too(see [[Layer Norm]])
[[Functions#^ce7dae]]
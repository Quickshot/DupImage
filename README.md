# DupImageLib

ImageDupLib is a .NET standard library offering several different perceptual hashing algorithms for detecting similar or duplicate images.

## Downloads

DupImageLib is available as a [Nuget package](https://www.nuget.org/packages/DupImageLib/). You can also download the Nuget packages from the [releases page](https://github.com/Quickshot/DupImageLib/releases).

## Features

DupImageLib implements the following perceptual hash algorithms:

- ### Average
  - Calculates the hash based on the average value of a scaled-down image. A more in-depth explanation of the algorithm can be found [here](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html). An extremely fast algorithm but generates a lot of false positives. Can generate 64 or 256-bit hashes.
- ### Median
  - Similar to average, but instead of average pixel value, the median value is used. This should make the algorithm bit more resistant to non-linear changes in the image. Slightly slower than average hash. Can generate 64 or 256-bit hashes
- ### Difference
  - Constructs the hash by comparing the gradients of pixel values on each row of the scaled image. In-depth explanation found [here](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html). Performs faster than the average hash and provides better results. Can generate 64 or 256-bit hashes.
- ### DCT (pHash)
  - Discrete cosine transform-based algorithm. Similar to the one implemented in [pHash library](http://www.phash.org/). A detailed explanation of the algorithm can be found [here](http://www.hackerfactor.com/blog/index.php?/archives/432-Looks-Like-It.html). Slower than any of the previous algorithms but has better tolerance to image modifications. Generates 64-bit hashes.

All algorithms accept the image input as a stream or as a path to a filesystem location.

DupImageLib also provides a function to compare hashes to return the similarity of the hashes.

DupImageLib uses [ImageSharp](https://github.com/SixLabors/ImageSharp) for image processing needs. Users of DupImageLib can use their own image processing library by providing an implementation of IImageTransformer and passing that to the ImageHashes constructor.

## Usage

### Example

```csharp
// Create new ImageHashes instance using ImageSharp as image manipulation library
var imageHasher = new ImageHashes(new ImageSharpTransformer());
// Calculate 64 bit hashes for the images using difference algorithm
var hash1 = imageHasher.CalculateDifferenceHash64(@"testimage1.png");
var hash2 = imageHasher.CalculateDifferenceHash64(@"testimage2.png");
// Calculate similarity between the hashes. Score of 1.0 indicates identical images.
var similarity = ImageHashes.CompareHashes(hash1, hash2);
```

## License

This software is licensed under Apache 2.0 license. See LICENSE file for more information.
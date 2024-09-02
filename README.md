# VoxelVideo Format

## Overview
The VoxelVideo format is a file structure designed specifically for the efficient handling, playback, and livestreaming of 3D voxel-based videos. This format is pivotal for applications in gaming, virtual reality, and interactive media, providing a robust framework for managing complex 3D data with spatial precision and color fidelity.

## File Structure
The current version of the VoxelVideo format is defined as follows, with plans to evolve into a more compressed non-JSON based format in future iterations:

```typescript
export default interface VoxelVideoData {
  Version: number;        // Specifies the version of the file format
  Framerate: number;      // Frames per second
  Framecount: number;     // Total number of frames
  Duration: number;       // Total duration of the video in seconds
  Dimensions: {           // Dimensions of the voxel matrix
    x: number;            // Width in voxels
    y: number;            // Height in voxels
    z: number;            // Depth in voxels
  };
  Title: string;          // Title of the video
  Blocks: (string | null)[][]; // 3D array representing the voxel grid
}
```

### Key Components
- **Version**: Helps in maintaining backward compatibility as the format evolves.
- **Framerate**, **Framecount**, and **Duration**: Provide essential playback information.
- **Dimensions**: Define the size of the voxel space to render the video correctly.
- **Title**: Identifies the video content.
- **Blocks**: A nested array where each inner array corresponds to a frame of video. Each frame consists of arrays that represent slices of the voxel space at various depths.

## Frame Types
- **I-frames (Intra-coded frames)**: Currently, every frame in the VoxelVideo format is an I-frame, which means that each frame is a complete voxel grid and does not require other frames to be decoded.
- **P-frames (Predicted frames)**: We are planning to introduce P-frames that will describe changes from one frame to another, reducing file size and improving streaming capabilities in 3D space.

## Reading the Blocks Array
To correctly read and interpret the `Blocks` array in each frame, follow this process:
1. Start at the top-left corner of the voxel grid at height 0.
2. Move from left to right across the row.
3. Upon reaching the end of the row, move down to the next row and repeat the left-to-right read.
4. After completing all rows at height 0, move up to the next plane and start the process over again.
5. Continue this pattern until all planes for the frame have been explored.

This method ensures that all voxels are read in a structured manner, maintaining the integrity of the 3D structure in the playback.

## Use-Case
The VoxelVideo format is tailored for scenarios requiring real-time interaction and manipulation of 3D video content, such as:
- Immersive live streamed virtual reality experiences.
- Detailed simulations and educational tools.
- Interactive gaming environments.

## Roadmap
- **Version 0.0** (Current): Initial release in JSON format, focusing on clarity and ease of use.
- **Future Versions**: Implementation of a compressed format to enhance performance and reduce file sizes, moving away from JSON to a binary format for efficiency at scale.

## Contributing
We welcome contributions from developers and enthusiasts. Please feel free to fork the repository, make your changes, and submit a pull request for review.

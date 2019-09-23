# flatten_all_movies_in_pwd
This script flattens movies from a maize rotational ear scanner into 2d cylindrical projections. It is presented as supplemental information to the paper "Custom built scanner and simple image processing pipeline enables low-cost, high-throughput phenotyping of maize ears" by Cedar Warman and John E Fowler.

## Requirements
FFMpeg and ImageMagick are required for this script to run.

## Detailed methods
Frames are first extracted from videos to png formatted images using the command line utility FFmpeg (https://ffmpeg.org) with default options (ffmpeg -i ./"$file" -threads 4 ./maize_processing_folder/output_%04d.png). These images are then cropped to the central row of pixels using the command line utility ImageMagick (https://imagemagick.org/) (mogrify -verbose -crop 1920x1+0+540 +repage ./maize_processing_folder/\*.png). The collection of single pixel row images is then appended in sequential order (convert -verbose -append +repage ./maize_processing_folder/\*.png ./"$name.png"). Finally, the image is rotated and cropped (mogrify -rotate "180" +repage ./"$name.png"; mogrify -crop 1920x746+0+40 +repage ./"$name.png"). We chose the convention of a horizontal flattened image with the top of the ear to the right and the bottom of the ear to the left. Because our videos were captured vertically, a rotation was required after appending the individual frames. Depending on the users video capture orientation and desired final image orientation, some modification may be necessary. The dimensions of the final crop of the image depend both on the rotation speed and the video frame rate. It is not necessary to capture one exact rotation, as long as the video captures at least one rotation. If the ears rotate at a consistent speed, one full rotation will always be the same number of frames. Because each frame becomes one pixel in height, the image can be cropped to a height corresponding to the number of frames that encompass one rotation. Because rotation speeds and frame rates vary, it is recommended that the user records the dimensions of an ear and compares those dimensions to the final output to ensure that there is no distortion. The final crop can be adjusted to address any possible distortion. For very high frame rates, the FFmpeg frame extraction rate can also be adjusted using the -vf option and adjusting the playback frames per second (e.g. to extract 10 frames per second: -vf fps=10).

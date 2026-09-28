# El Ojo del Árbitro Digital

Computer vision assignment: take a random frame from a football match video and mark, when they appear:

- Players: blue contour
- Ball: red contour
- Field lines: green

If an element is not in the frame, nothing is drawn for it.

Only techniques seen in class are used: color filtering on the RGB channels, binary masks and logical operations, Gaussian filter, LoG, Canny and Region Growing.

## How to run

1. Upload `partidoArgVsBrasil_h264.mp4` to your Google Drive.
2. Open `Ojo_del_Arbitro_Digital.ipynb` in Google Colab.
3. Set `VIDEO_PATH` to the video's location in Drive and run all cells.

Each run saves the result as `result_YYYYMMDD_HHMMSS.png` in the same Drive folder as the video.

## Video

Argentina vs Brazil. The original download was AV1, which OpenCV can't read, so it was converted to H.264 (720p) with ffmpeg.

## Tests

`tests/` has the output for 30 random frames of the video.

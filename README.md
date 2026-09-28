# El Ojo del Árbitro Digital

Class assignment (trabajo práctico) for the Computer Vision course (Visión Computacional).

The notebook takes a random frame from a football match video and marks players in blue, the ball in red and the field lines in green. Anything that is not in the frame is left unmarked.

## Assignment

Build a computer vision system in Google Colab that processes a football match video, takes a random frame and highlights the key elements of the game when they appear in it:

- Players: a blue contour around each detected player. Individual tracking is not needed.
- Ball: a red contour around the ball, if it is in the frame.
- Field limits: the field lines (sidelines, goal lines, boxes) highlighted in green.

If none of the elements appear in the frame, nothing is marked. The work is graded on the correct use of the algorithms seen in class (edge detection, color filtering, segmentation) and on delivering a working Colab with the video used. Each team works with a different video.

## Architecture

```mermaid
flowchart TD
    A[Video] --> B[Random frame]
    B --> C[Grass mask<br/>RGB channel rules + AND]
    C --> D[Field region<br/>Region Growing from a seed]
    D -->|no field| Z[Frame without marks]
    D --> E[Field lines<br/>white + LoG + Canny]
    D --> F[Objects on the field<br/>field AND NOT grass AND NOT lines]
    E --> F
    F --> G[Players<br/>size and shape rules]
    F --> H[Ball<br/>small, round, bright, surrounded by grass]
    G --> H
    E --> I[Draw: lines green, players blue, ball red]
    G --> I
    H --> I
    I --> J[result_YYYYMMDD_HHMMSS.png]
```

## How to run

1. Download the video (see [Video](#video)), convert it to H.264 and upload it to your Google Drive.
2. Open `Ojo_del_Arbitro_Digital.ipynb` in Google Colab.
3. Set `VIDEO_PATH` to the video's location in Drive and run all cells.

Each run saves the result as `result_YYYYMMDD_HHMMSS.png` in the same Drive folder as the video. Set `SEED` to a number to repeat the same frame.

## How it was built

Only techniques seen in class are used: color filtering on the RGB channels, binary masks and logical operations, Gaussian and mean filters, LoG, Canny and Region Growing. OpenCV is used to read the video and to find and draw contours.

- Grass: green has to exceed red and blue by a margin. Pixels that are too bright, too blue or too red are excluded, so the ball, Brazil's yellow shirt and the goalkeeper's neon kit don't count as grass.
- Field: Region Growing (4-connectivity) from a seed near the bottom center keeps only the pitch, not green billboards or stands. If the region is too small or doesn't reach the bottom edge, the frame is not a game shot and nothing is drawn. The broadcast logo and scoreboard are masked out.
- Lines: light pixels that are also thin (negative LoG) and close to a Canny edge. Short or thick regions are dropped.
- Players: what is on the field and is neither grass nor line, joined with a few mask growing passes and filtered by height, area and shape. The outline is grown a few pixels so it doesn't cut the player.
- Ball: a small, round, bright region surrounded by grass, not touching a line or a player.

All thresholds are in the `P` dictionary at the top of the notebook. They were tuned on random frames of this video.

## Video

Argentina vs Brazil: https://www.youtube.com/watch?v=Uk9ud7UXJYM&t=24s

The video is not in the repo because of its size. The download was AV1, which OpenCV can't read, so it was converted to H.264 (720p) with ffmpeg.

## Tests

`tests/test_1` and `tests/test_2` each have the output for 30 random frames of the video (two separate runs).

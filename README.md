# Self-referential pixel art in  GitHub commit history calendar with custom 2x4 font
## Repository for pixel art in GitHub commit history calendar.

### 1. Prelude
Some time ago I was watching a [video by carykh](https://youtu.be/uMogvkogvhE) about self-referential sentences and graphs, and it inspired me to make something similar, but in GitHub's commit history calendar. If you visit any user profile you can find a counter, how many commits this person made in a given year as well as draws daily calendar displaying those commits. My idea was to draw total number of commits for year 2024 with a really small bitmap font. As you can guess number needs to include both normal commits as well as "fake" ones for the pixel art itself, that's what's called self-referential graph. Thankfully it's a really easy problem to solve with brute force by iterating all numbers and counting how many pixels it would take :)


### 2. Pixel art
I wanted to use an extremely small font. If you search for smallest possible bitmap fonts you will find that there are basically no good ones, so I went on a quest to make my own.

Goal was to make full ASCII table as well as Cyrillic, each symbol should of course be unique so you need to at least have ability to make 153 combinations with your binary pixels. Smallest power of two that fits that number is `2^8 = 256`, meaning that you need 8 pixels in total. Since most glyps in my goal alphabet are taller than they are wide I chose size of 2x4 pixels for each glyph.

Now it is time to make the font. I wanted to automate the process as much as possible. Sadly experiments with upscaling randomly generated 8-bit numbers as 2x4 images with Signed Distance Fields (SDF) and trying to find the closest matches with "normal" fonts didn't produce good results. I guess a valid approach would be trying to teach some small diffusion model to generate you really small glyphs, but that is out of the scope of this project.

I went with simpler approach. I generated all possible combinations of 8 pixels and drew this texture:

<p align="center">
  <img src="https://github.com/user-attachments/assets/cdc2b890-9ac2-4fd6-92e0-4f55f7ee6f59" alt="All possible 2x4 bitmaps texture">
</p>

Next I parsed shakespeare (as you always do for such tasks xd) and war and peace and ordered my target symbols from most popular to least popular. Next I was going down that list manually and trying to find fitting glyphs in the texture I generated earlier. I was trying to pick N possibilities ordered from best fitting one to the least fitting one.

After I had a lond list where each target glyph had 1-8 variations how it could be drawn in a 2x4 space I coded up a simple version of [Wave Function Collapse (WFC)](https://en.wikipedia.org/wiki/Wave_function_collapse) algorithm to pick best fitting glyphs but also comply with a rule that each one should be unique.

Font is still in progress and I will not share full one, but here are the numbers I ended up with:

<p align="center">
  <img src="https://github.com/user-attachments/assets/a4d65391-55f6-4ebd-a743-158130ca49e0" alt="Numbers">
</p>

### 3. Drawing pixel art in GitHub commit history calendar
Obviously putting weekly alarm at a select dates to commit something to make a pixel art is not an easy task. Thankfully GitHub decided it's a good idea to trust client with providing timestamps in commit requests. Whenever you commit something, git puts a Unix Timestamp on it to record when you committed it.

I used this tool by gelstudios called [gitfiti](https://github.com/gelstudios/gitfiti). It helps generating bash script to commit in needed dates for a set bitmap. For the target number of 139 commits in year 2024, here is the input:
```json
:one-hundred-thirty-nine
[[0,0,0,0,0,0,0,0],
[0,0,0,0,0,0,0,0],
[0,1,0,1,0,0,1,1],
[1,1,0,1,1,0,1,1],
[0,1,0,0,1,0,0,1],
[0,1,0,1,0,0,1,0],
[0,0,0,0,0,0,0,0]]

```

### 4. Final result
Final result can be seen on [my GitHub profile](https://github.com/ebolblga?tab=overview&from=2024-12-01&to=2024-12-31).

![image](https://github.com/user-attachments/assets/796a144d-9308-49a4-a89f-967ac678a58b)

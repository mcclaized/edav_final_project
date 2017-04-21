# edav_final_project

## Intro

You can skip all of the **Setup** and **Usage** if you just want to see the picture. Clone this repository and drag `edav_final_project/example/index.html` into Firefox.

*Note: other browsers may not work due to restrictions in the `d3.json()` call referencing the filesystem instead of HTTP*

## Addendum to the Main Analysis section

I realized that there was one key point that I did not make in the **Main Analysis** section of my write up that I think is important. It's easiest to follow along if you have the `fibonacci` example visualization open in your browser or are looking at the screenshots in the **Appendix**.

Take a look at the FlameGraph titled "New Profile", and imagine that you are a developer who recently made the code changes that resulted in this image. Without having either of the other two profiles available for context, what would you think? In my opinion, it's very hard to see whether the code was improved or regressed. 

A regular FlameGraph (like http://queue.acm.org/downloads/2016/Gregg4.svg) chooses a warm color palette for each rectangle at random, so the color only serves to differentiate the different function calls. In "New Profile", the colors represent the number of function calls relative to the "Baseline" profile (more calls in New Profile relative to Baseline --> Red, fewer calls in New Profile --> Blue), but even this isn't very helpful in this case: there seems to be an even mix of blue and red and what's even more alarming is that the parts that are most red are the ones that contain calls to our `fibonacci_improved.py` source code!

If we can take a step back and look at the other two graphs it becomes immediately clear that we made a huge improvement: we dramatically reduced the total number of calls. The reason we couldn't see this in the "New Profile" is that the rectangles are determined only by the calls made in the *improved* program, so it has no way of showing what was removed.

Clearly, the "Baseline" and "New Profile" together show two halves of the whole, but I'd also like to point out the "Profile Delta". In this example, you probably don't need the "Profile Delta" because the differences between Baseline and New are so stark. But in cases where you are profiling a large codebase and only making small changes, it is reasonable to assume that Baseline and New Profile would look largely the same.

See the two Pull Requests mentioned below for details on how I implemented this "timeshare" feature.

End of Addendum, thanks for reading!

## Setup instructions

### Clone this repository

```bash
git clone https://github.com/mcclaized/edav_final_project.git
```

### Clone my FlameGraph fork
(see https://github.com/mcclaized/FlameGraph/pull/1 for details on what I changed)
```bash
git clone https://github.com/mcclaized/FlameGraph.git
cd FlameGraph
git checkout diff_two_flamegraphs
cd ..
```

### Clone my node-stack-convert fork 
(see https://github.com/mcclaized/node-stack-convert/pull/1 for details on what I changed)

```bash
git clone https://github.com/mcclaized/node-stack-convert.git
cd node-stack-convert
git checkout render_flamegraph_diff
cd ..
```

### Install pyflame (Linux Only)

#### On Ubuntu:
```bash
sudo apt-add-repository ppa:trevorjay/pyflame
sudo apt-get update
sudo apt-get install pyflame
```

## Usage

### Write some python code to test:
#### Example code taken from https://pymotw.com/2/profile/
`example/scripts/fibonacci_bad.py`

`example/scripts/fibonacci_improved.py`

### Generate some "folded" profiles

```bash
edav_final_project/bin/run_pyflame python2.7 fibonacci_bad.py > example/data/fibonacci_bad.folded
edav_final_project/bin/run_pyflame python2.7 fibonacci_improved.py > fibonacci_improved.folded
```

### Create diffs of the profiles

```bash
FlameGraph/difffolded.pl fibonacci_bad.folded fibonacci_improved.folded > edav_final_project/example/data/baseline_diff.folded
FlameGraph/difffolded.pl fibonacci_improved.folded fibonacci_bad.folded > edav_final_project/example/data/improved_diff.folded
FlameGraph/difffolded.pl -u edav_final_project/example/data/baseline_diff.folded edav_final_project/example/data/improved_diff.folded > edav_final_project/example/data/delta_diff.folded
```

### Transform to JSON for d3.js

```bash
node-stack-convert/index.js -f edav_final_project/example/data/baseline_diff.folded > edav_final_project/example/data/baseline_diff.json
node-stack-convert/index.js -n -f edav_final_project/example/data/improved_diff.folded > edav_final_project/example/data/improved_diff.json
node-stack-convert/index.js -f edav_final_project/example/data/delta_diff.folded > edav_final_project/example/data/delta_diff.json
```

### Load Visualization!

Drag and drop `edav_final_project/example/index.html` into **Firefox** (note: other browsers may not work due to restrictions in the `d3.json()` call coming from the filesystem)

## Links / Attribution
- http://queue.acm.org/detail.cfm?id=2927301
- http://www.brendangregg.com/blog/2014-11-09/differential-flame-graphs.html
- https://github.com/corpaul/flamegraphdiff
- `index.html` modeled after https://github.com/spiermar/d3-flame-graph/blob/master/example/index.html

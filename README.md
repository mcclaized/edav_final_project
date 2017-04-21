# edav_final_project

## Intro

You can skip all of the **Setup** and **Usage** if you just want to see the picture. Clone this repository and drag `edav_final_project/example/index.html` into Firefox.

*Note: other browsers may not work due to restrictions in the `d3.json()` call referencing the filesystem instead of HTTP*

## Setup instructions

### Clone this repository

```bash
git clone https://github.com/mcclaized/edav_final_project.git
```

### Clone my FlameGraph fork

```bash
git clone https://github.com/mcclaized/FlameGraph.git
cd FlameGraph
git checkout diff_two_flamegraphs
cd ..
```

### Clone my node-stack-convert fork

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

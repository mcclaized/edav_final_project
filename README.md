# edav_final_project

##Intro


## Setup instructions

### Install Flamegraph

```bash
curl https://github.com/brendangregg/FlameGraph/archive/master.zip > FlameGraph.zip
unzip FlameGraph.zip
mv FlameGraph-master FlameGraph
```

### Install Flamegraph Fork

```bash
curl https://github.com/corpaul/FlameGraph/archive/29b0bf01d062646b17659a37c70a7795cf742019.zip > FlameGraphFork.zip
unzip FlameGraphFork.zip
mv FlameGraph-29b0bf01d062646b17659a37c70a7795cf742019 FlameGraphFork
```

### Install pyflame

```bash
sudo apt-add-repository ppa:trevorjay/pyflame
sudo apt-get update
sudo apt-get install pyflame
```

## Usage

Write some python code to test:

fibonacci_bad.py
fibonacci_improved.py

Generate some graphs

pyflame -t python2.7 fibonacci_bad.py > fibonacci_bad.folded
pyflame -t python2.7 fibonacci_improved.py > fibonacci_improved.folded

Create diffs

FlameGraph/difffolded.pl fibonacci_bad.folded fibonacci_improved.folded > did_happen.folded
FlameGraph/flamegraph.pl did_happen.folded > did_happen.svg

FlameGraph/difffolded.pl fibonacci_improved.folded fibonacci_bad.folded > will_happen.folded
FlameGraph/flamegraph.pl --negate will_happen.folded > will_happen.svg

FlameGraphFork/difffolded.pl -d fibonacci_bad.folded fibonacci_improved.folded > diff.folded
FlameGraph/flamegraph.pl diff.folded > diff.svg


## Links / Attribution

http://www.brendangregg.com/blog/2014-11-09/differential-flame-graphs.html

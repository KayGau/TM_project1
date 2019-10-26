### How to run:
Generate storyline data:
```python 
python pytorch_src/extract.py
```
Train title to storyline model 
```Python
Python pytorch_src/main.py --batch_size 20 --train-data pytorch_src/data/title2line.train --valid-data pytorch_src/data/title2line.val --test-data pytorch_src/data/title2line.test --dropouti 0.4 --dropouth 0.25 --seed 141 --epoch 100 --emsize 200 --nhid 300 --save title2line_model.pt --vocab-file vocab.pkl --cuda
```
Train title and storyline to story model
```Python
Python pytorch_src/main.py --batch_size 20 --train-data pytorch_src/data/title2line2story.train --valid-data pytorch_src/data/title2line2story.val --test-data pytorch_src/data/title2line2story.test --dropouti 0.4 --dropouth 0.25 --seed 141 --epoch 100 --emsize 200 --nhid 300 --save title2line2story_model.pt --vocab-file vocab2.pkl --cuda
```
Generate storyline from title
```Python
Python pytorch_src/generate.py --checkpoint title2line_model.pt --vocab vocab.pkl --task cond_generate --conditional-data pytorch_src/data/title.test --cuda --temperature 0.5 --sents 8161 --dedup --outf storyline.test
```
Generate story from title and storyline
```bash
$ paste -d ' ' pytorch_src/data/title.test storyline.test > title_line.test
```
```Python
Python pytorch_src/generate.py --vocab vocab2.pkl --checkpoint title2line2story_model.pt --task cond_generate --conditional-data title_line.test --cuda --temperature 0.3 --sents 8161 --outf story.test
```

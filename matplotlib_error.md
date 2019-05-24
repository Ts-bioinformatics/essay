### source article: [Here](https://www.e-learn.cn/content/wangluowenzhang/823897)
## Question:
```
import os
import matplotlib
import matplotlib.pyplot as plt

...
```
I run this script in background of server computer, then I get this error:
```
Traceback (most recent call last):
  File "/work1/bigdata/webroot/apache-tomcat-compute-37542/webapps/calculink01/calculink_data_space/unit/calculink01.user/blood/file/My_data/my_scripts/sunshx_scripts/Palantir/Palantir_model.py", line 29, in <module>
    fig, ax = palantir.plot.plot_molecules_per_cell_and_gene(counts)
  File "/work1/software/x86_64.1/Python/3.6.8/lib/python3.6/site-packages/palantir/plot.py", line 111, in plot_molecules_per_cell_and_gene
    fig = plt.figure(figsize=[width, height])
  File "/work1/software/x86_64.1/Python/3.6.8/lib/python3.6/site-packages/matplotlib/pyplot.py", line 539, in figure
    **kwargs)
  File "/work1/software/x86_64.1/Python/3.6.8/lib/python3.6/site-packages/matplotlib/backend_bases.py", line 3252, in new_figure_manager
    return cls.new_figure_manager_given_figure(num, fig)
  File "/work1/software/x86_64.1/Python/3.6.8/lib/python3.6/site-packages/matplotlib/backends/_backend_tk.py", line 946, in new_figure_manager_given_figure
    window = tk.Tk(className="matplotlib")
  File "/work1/software/x86_64.1/Python/3.6.8/lib/python3.6/tkinter/__init__.py", line 2023, in __init__
    self.tk = _tkinter.create(screenName, baseName, className, interactive, wantobjects, useTk, sync, use)
_tkinter.TclError: couldn't connect to display "localhost:19.0"
```

## Answer:
```
Generate images without having a window appear (background)
```
use a non-interactive backend (see What is a [backend](https://matplotlib.org/faq/usage_faq.html#what-is-a-backend)?) such as Agg (for PNGs), PDF, SVG or PS. 
In your figure-generating script, just call the matplotlib.use() directive before importing pylab or pyplot:
```
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

plt.plot([1,2,3])
plt.savefig('myfig')
```

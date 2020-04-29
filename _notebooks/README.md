# Auto-convert Jupyter Notebooks To Posts

[`fastpages`](https://github.com/fastai/fastpages) will automatically convert [Jupyter](https://jupyter.org/) Notebooks saved into this directory as blog posts!

You must save your notebook with the naming convention `YYYY-MM-DD-*.ipynb`.  Examples of valid filenames are:

```shell
2020-01-28-My-First-Post.ipynb
2012-09-12-how-to-write-a-blog.ipynb
```

If you fail to name your file correctly, `fastpages` will automatically attempt to fix the problem by prepending the last modified date of your notebook. However, it is recommended that you name your files properly yourself for more transparency.

See [Writing Blog Posts With Jupyter](https://github.com/fastai/fastpages#writing-blog-posts-with-jupyter) for more details.

## 常见问题

### 显示plotly图

参考：

- https://covid19dashboards.com/covid-overview-morocco/
- https://colab.research.google.com/github/github/covid19-dashboard/blob/master/_notebooks/2020-03-30-us-inflection.ipynb

```python
#hide_output
# plotly-express
from IPython.display import HTML
fig = px.pie(by_regions, values='Confirmed', names='Region', title="Covid19 Cases distribution", color_discrete_sequence=px.colors.sequential.RdBu)
fig.show()
```

```python
#hide_output
# cufflinks
from IPython.display import HTML
fig = cf.gendata.lines(4).iplot(asFigure=True)
fig.show()
```

```python
#hide_input
HTML(fig.to_html())
```

### 表格背景自动填充

`df.style.background_gradient(cmap='Reds')`

### 显示地图

```python
#hide
# map.save('morocco-map.html')
# map.write_image('covid-compare-1-2.png')


# HTML(filename="morocco-map.html")
# map
import IPython

HTML('morocco-map.html')
url = 'morocco-map.html'
iframe = '<iframe src=' + url + ' width=900 height=600></iframe>'
HTML(iframe)

html_string = map.get_root().render()
```

### 脚注的正确使用方法

- https://github.com/fastai/fastpages/blob/master/_fastpages_docs/NOTEBOOK_FOOTNOTES.md

## 好的demo

- https://drscotthawley.github.io/devblog3/2019/12/21/PCA-From-Scratch.html#Covariance

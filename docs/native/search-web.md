# 网页搜索

> 百度网页搜索，也可以作为综合搜索使用。

```python
BaiduSpider.search_web(self: BaiduSpider, query: str, pn: int = 1, exclude: list = []) -> dict
```

## 参数

- self: BaiduSpider，即必须实例化`BaiduSpider`对象后才可调用。

- query: str，要查询网页搜索的字符串

- pn: int，要爬取的页码，默认为1，为可选参数

- exclude: dict，要屏蔽的子部件列表，为可选参数

### 实例

#### 基本的调用

```python hl_lines="9"
# 导入BaiduSpider
from baiduspider import BaiduSpider
from pprint import pprint

# 实例化BaiduSpider
spider = BaiduSpider()

# 搜索网页
pprint(spider.search_web(query=input('要搜索的关键词：')))
```

#### 指定页码

```python hl_lines="7"
from baiduspider import BaiduSpider
from pprint import pprint

spider = BaiduSpider()

# 搜索网页，并传入页码参数
pprint(spider.search_web(query=input('要搜索的关键词：'), pn=int(input('页码：'))))
```

!!! warning
    传入页码参数的时候一定要小心，务必不要传入过大的页码，因为百度搜索会自动跳转回第一页

#### 屏蔽特定的搜索结果

```python hl_lines="8"
from baiduspider import BaiduSpider
from pprint import pprint

spider = BaiduSpider()

# 搜索网页，并传入要屏蔽的结果
# 在本样例中，屏蔽了贴吧和博客
pprint(spider.search_web(query=input('要搜索的关键词：'), exclude=['tieba', 'blog']))
```

`exclude`的值可以包含：`['news', 'video', 'baike', 'tieba', 'blog', 'gitee', 'related', 'calc']`，分别表示：资讯，视频，百科，贴吧，博客，Gitee代码仓库，相关搜索，计算。`exclude`的值也可以是`['all']`，表示屏蔽除了普通搜索结果外的所有搜索结果。实例：

```python hl_lines="8"
from baiduspider import BaiduSpider
from pprint import pprint

spider = BaiduSpider()

# 搜索网页，并传入要屏蔽的结果
# 在本样例中，屏蔽了所有非普通的搜索结果
pprint(spider.search_web(query=input('要搜索的关键词：'), exclude=['all']))
```

如果`exclude`中包含`all`且还有其他参数，那么将按照只有`all`的方式过滤搜索结果。

## 返回值

`BaiduSpider.search_web`函数的返回类型是`dict`，大致返回模版如下：

```python
{
    # 搜索结果列表
    'results': [
        # 总计搜索结果数量
        {
            'result': int,  # 总计搜索结果数
            'type': 'total'  # type用来区分不同类别的搜索结果
        },
        # 相关搜索建议
        {
            'results': [str],  # 相关搜索建议
            'type': 'related'
        },
        # 运算窗口，仅会在搜索词涉及运算时出现
        {
            'process': str,  # 算数过程
            'result': str,  # 运算结果
            'type': 'calc'
        },
        # 相关新闻，仅会在搜索词有相关新闻时出现
        {
            'results': [{
                'author': str,  # 新闻来源
                'time': str,  # 新闻发布时间
                'title': str,  # 新闻标题
                'url': str,  # 新闻链接
                'des': str  # 新闻简介，大部分时间是None
            }],
            'type': 'news'
        },
        # 相关视频，仅会在搜索词有相关视频时出现
        {
            'results': [{
                'cover': str,  # 视频封面图片链接
                'origin': str,  # 视频来源
                'length': str,  # 视频时长
                'title': str,  # 视频标题
                'url': str  # 视频链接
            }],
            'type': 'video'
        },
        # 相关百科，仅会在搜索词有相关百科时出现
        {
            'result': {
                'cover': str,  # 百科封面图片/视频链接
                'cover-type': str,  # 百科封面类别，图片是image，视频是video
                'des': str,  # 百科简介
                'title': str,  # 百科标题
                'url': str  # 百科链接
            },
            'type': 'baike'
        },
        # 相关贴吧，仅会在搜索词有相关贴吧时出现
        {
            'result': {
                'cover': str,  # 贴吧封面图片链接
                'des': str,  # 贴吧简介
                'title': str,  # 贴吧标题
                'url': str,  # 贴吧链接
                'followers': str,  # 贴吧关注人数（可能有汉字，如：1万）
                'hot': [{  # list, 热门帖子
                    'clicks': str,  # 帖子点击总数
                    'replies': str,  # 帖子回复总数
                    'title': str,  # 帖子标题
                    'url': str, #  帖子链接
                }],
                'total': str,  # 贴吧总帖子数（可能有汉字，如：17万）
            },
            'type': 'tieba'
        },
        # 相关博客，仅会在搜索词有相关博客时出现
        {
            'result': {
                'blogs': [{  # list, 博客列表
                    'des': str,  # 博客简介，没有时为`None`
                    'origin': str,  # 博客来源
                    'tags': [  # list, 博客标签
                        str,  # 标签文字
                    ],
                    'title': str,  # 博客标题
                    'url': str,  # 博客链接
                }],
                'title': str,  # 博客搜索标题
                'url': str,  # 博客搜索链接 (https://kaifa.baidu.com)
            },
            'type': 'blog'
        },
        # 相关Gitee仓库，仅会在搜索词有相关Gitee仓库时出现
        {
            'result': {
                'title': str,  # 仓库标题
                'des': str,  # 仓库简介
                'url': str,  # 仓库链接
                'star': int,  # 仓库star数
                'fork': int,  # 仓库fork数
                'watch': int,  # 仓库watch数
                'license': str,  # 仓库版权协议
                'lang': str,  # 仓库使用的编程语言
                'status': str,  # 仓库状态图表链接
            },
            'type': 'gitee'
        },
        # 普通的搜索结果
        {
            'des': str,  # 搜索结果简介
            'origin': str,  # 搜索结果的来源，可能是域名，也可能是名称
            'title': str, # 搜索结果标题
            'type': 'result',
            'url': str # 搜索结果链接
        }
    ],
    # 总计的搜索结果页数，可能会因为当前页数的变化而随之变化
    'total': int
}
```

!!! note
    因为百度网页搜索有多种搜索结果，所以`search_web`函数使用`type`标签为不同类别的搜索结果分类，如视频是`video`，资讯是`news`，运算是`calc`等不同标签，其中`result`为普通的搜索结果。

## 返回值解释

在返回值中，有两个键，分别是`results`和`total`。其中，`total`表示总计搜索结果的页数，并且可能会因为当前页数的变化而随之变化，值类型为`int`。`results`代表搜索结果列表，类型为`list`。下面是列表中各个值的详细解释。

### 普通的搜索结果

该项的分类为`result`，模型如下：

```python
{
    'des': str,
    'origin': str,
    'title': str,
    'type': 'result',
    'url': str
}
```

#### 解释

- `des`：搜索结果简介，类型为`str`
- `origin`：搜索结果来源，类型为`str`
- `title`：搜索结果标题，类型为`str`
- `type`：该项表示该结果的类别，值为`result`，类型是`str`
- `url`：搜索结果链接，类型为`str`

!!!note
    普通搜索结果没有一个父字典包围着，它的父字典就是返回值中的`results`字典

### 结果数量

该项的分类为`total`，模型如下：

```python
{
    'result': int,
    'type': 'total'
}
```

#### 解释

- `result`：该项表示总计的搜索结果数量（大约），值类型为`int`
- `type`：该项表示该结果的类别，值为`total`，类型是`str`

### 相关搜索建议

该项的分类为`related`，模型如下：

```python
{
    'results': [str],
    'type': 'related'
}
```

#### 解释

- `results`：该项中每个成员类型为`str`，表示一个相关搜索，值类型为`list`
- `type`：该项表示该结果的类别，值为`related`，类型是`str`

### 运算

该项的分类为`calc`，模型如下：

```python
{
    'process': str,
    'result': str,
    'type': 'calc'
}
```

#### 解释

- `process`：该项为运算过程，如`10*9-8`，值类型为`str`
- `result`：该项为运算结果，如`82`，值类型为`str`
- `type`：该项表示该结果的类别，值为`calc`，类型是`str`

!!!warning
    本项搜索结果仅在搜索词参与运算时出现，请谨慎调用


### 相关新闻 ^BETA^

该项的分类为`news`，模型如下：

```python
{
    'results': [{
        'author': str,
        'time': str,
        'title': str,
        'url': str,
        'des': str
    }],
    'type': 'news'
}
```

#### 解释

- `results`：表示相关新闻，其中有许多子字典，每个子字典表示一条新闻。值类型为`list`
    - `author`：新闻来源，类型为`str`
    - `time`：新闻发布时间，类型为`str`
    - `title`：新闻标题，类型为`str`
    - `url`：新闻链接，类型为`str`
    - `des`：新闻简介，类型为`str`，大部分时间为None
- `type`：该项表示该结果的类别，值为`news`，类型是`str`

!!!warning
    本项搜索结果仅在搜索词有相关新闻时出现，请谨慎调用

### 相关视频 ^BETA^

该项的分类为`video`，模型如下：

```python
{
    'results': [{
        'cover': str,
        'origin': str,
        'length': str,
        'title': str,
        'url': str
    }],
    'type': 'video'
}
```

#### 解释

- `results`：表示相关视频，其中有若干个子字典，每个子字典表示一个相关视频。值类型为`list`
    - `cover`：视频封面图片链接，值类型为`str`
    - `origin`：视频来源，值类型为`str`
    - `length`：视频时长，值类型为`str`
    - `title`：视频标题，值类型为`str`
    - `url`：视频链接，值类型为`str`
- `type`：该项表示该结果的类别，值为`video`，类型是`str`

!!!warning
    本项搜索结果仅在搜索词有相关视频时出现，请谨慎调用

### 相关百科 ^BETA^

该项的分类为`baike`，模型如下：

```python
{
    'result': {
        'cover': str,
        'cover-type': str,
        'des': str,
        'title': str,
        'url': str
    },
    'type': 'baike'
},
```

#### 解释

- `result`：表示一个百科，值类型为`dict`
    - `cover`：百科的封面链接，可能时图片，也可能是视频，值类型为`str`
    - `cover-type`：百科的封面类型，图片为`image`，视频为`video`，值类型为`str`
    - `des`：百科简介，值类型为`str`
    - `title`：百科标题，值类型为`str`
    - `url`：百科链接，值类型为`str`
- `type`：该项表示该结果的类别，值为`baike`，类型是`str`

!!!warning
    本项搜索结果仅在搜索词有相关百科时出现，请谨慎调用

### 相关贴吧 ^ALPHA^

该项的分类为`tieba`，模型如下：

```python
{
    'result': {
            'cover': str,
            'des': str,
            'title': str,
            'url': str,
            'followers': str,
            'hot': [{
                'clicks': str,
                'replies': str,
                'title': str,
                'url': str
            }],
            'total': str
    },
    'type': 'tieba'
}
```

#### 解释

- `result`：表示贴吧结果字典，类型为`dict`
    - `cover`：贴吧封面图片链接，类型为`str`
    - `des`：贴吧简介，类型为`str`
    - `title`：贴吧标题，类型为`str`
    - `url`：贴吧链接，类型为`str`
    - `followers`：贴吧关注人数（可能有汉字，如：1万），类型为`str`
    - `hot`：贴吧热门帖子，类型为`list`
        - `clicks`：帖子点击总数，类型为`str`
        - `replies`：帖子回复总数，类型为`str`
        - `title`：帖子标题，类型为`str`
        - `url`：帖子链接，类型为`str`
    - `total`：贴吧总帖子数（可能有汉字，如：17万），类型为`str`
- `type`：该项表示该结果的类别，值为`tieba`，类型是`str`

### 相关博客 ^ALPHA^

该项的分类为`blog`，模型如下：

```python
{
    'result': {
        'blogs': [{
            'des': str,
            'origin': str,
            'tags': [
                str
            ],
            'title': str,
            'url': str
        }],
        'title': str,
        'url': str
    },
    'type': 'blog'
}
```

#### 解释

- `result`：表示博客结果字典，类型为`dict`
    - `blogs`：表示博客列表，类型为`list`
        - `des`：表示博客简介，没有时为`None`，类型为`str`
        - `origin`：表示博客来源，类型为`str`
        - `tags`：表示博客标签列表，类型为`list`
            - `tags`<sub>`i`</sub>：表示一个博客标签，类型为`str`
        - `title`：表示博客标题，类型为`str`
        - `url`：表示博客链接，类型为`str`
    - `title`：表示博客搜索标题，类型为`str`
    - `url`：表示博客搜索链接 (<https://kaifa.baidu.com>)，类型为`str`

### 相关Gitee仓库 ^ALPHA^

该项的分类为`gitee`，模型如下：

```python
{
    'result': {
        'title': str,
        'des': str,
        'url': str,
        'star': int,
        'fork': int,
        'watch': int,
        'license': str,
        'lang': str,
        'status': str
    },
    'type': 'gitee'
}
```

#### 解释

- `result`：表示Gitee仓库结果字典，类型为`dict`
    - `title`：表示仓库标题，类型为`str`
    - `des`：表示仓库简介，类型为`str`
    - `url`：表示仓库链接，类型为`str`
    - `star`：表示仓库star数，类型为`int`
    - `fork`：表示仓库fork数，类型为`int`
    - `watch`：表示仓库watch数，类型为`int`
    - `license`：表示仓库版权协议，类型为`str`
    - `lang`：表示仓库使用的编程语言，类型为`str`
    - `status`：表示仓库的状态图表链接，类型为`str`

## 提醒

!!!warning
    不要使用爬虫搜索大量信息（如每天1000条），这样可能导致被百度封锁IP地址。你也不能将本项目用于商业用途。

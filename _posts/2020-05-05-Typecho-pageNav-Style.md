---
layout: post
title: "typecho 的分页函数 pageNav 修改分页样式"
date: 2020-05-05 03:51:16
---


pageNav 函数出现在两个地方：
```
    /var/Widget/Archive.php
    /var/Widget/Comments/Archive.php
```
函数内容：
```
        /**
         * 输出分页
         *
         * @access public
         * @param string $prev 上一页文字
         * @param string $next 下一页文字
         * @param int $splitPage 分割范围
         * @param string $splitWord 分割字符
         * @param string $template 展现配置信息
         * @return void
         */
        public function pageNav($prev = '«', $next = '»', $splitPage = 3, $splitWord = '...', $template = '')
        {
            if ($this->have()) {
                $hasNav = false;
                $default = array(
                    'wrapTag'       =>  'ol',
                    'wrapClass'     =>  'page-navigator'
                );

                if (is_string($template)) {
                    parse_str($template, $config);
                } else {
                    $config = $template;
                }

                $template = array_merge($default, $config);
                
                $total = $this->getTotal();
                $this->pluginHandle()->trigger($hasNav)->pageNav($this->_currentPage, $total,
                    $this->parameter->pageSize, $prev, $next, $splitPage, $splitWord);

                if (!$hasNav && $total > $this->parameter->pageSize) {
                    $query = Typecho_Router::url($this->parameter->type .
                    (false === strpos($this->parameter->type, '_page') ? '_page' : NULL),
                    $this->_pageRow, $this->options->index);

                    /** 使用盒状分页 */
                    $nav = new Typecho_Widget_Helper_PageNavigator_Box($total,
                        $this->_currentPage, $this->parameter->pageSize, $query);
                   
                    echo '<' . $template['wrapTag'] . (empty($template['wrapClass'])
                        ? '' : ' class="' . $template['wrapClass'] . '"') . '>';
                    $nav->render($prev, $next, $splitPage, $splitWord, $template);
                    echo '</' . $template['wrapTag'] . '>';
                }
            }
        }

```

如果不在乎样式，可以直接使用：
```
    <?php $this->pageNav(); ?>

```
在模板的archive.php里是这样用的：
```
    <?php $this->pageNav('« 前一页', '后一页 »'); ?>
```
根据上面函数里的描写：
```
    $nav->render($prev, $next, $splitPage, $splitWord, $template);

```
用法应该是这样的：
```
    <?php $this->pageNav('前一页', '后一页','省略符','样式模板'); ?>

```
参数都有严格的先后顺序，搞错了显示就错位，读者可以试一下。

而跟css有关的，就是最后一个“样式模板” - $template。

完整的使用方法：
```
    <?php $this->pageNav('前一页', '后一页', 1, '...', array('wrapTag' => 'ul', 'wrapClass' => 'pagination', 'itemTag' => 'li', 'textTag' => 'span', 'currentClass' => 'current', 'prevClass' => 'prev', 'nextClass' => 'next',)); ?>
```
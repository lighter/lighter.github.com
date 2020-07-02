---
layout: post
title: "php oracle oci_bind_by_name"
date: 2014-04-29 23:35
comments: true
tags: 
- php
- oracle
---

原本使用`Codeigniter`的`bind`很簡單！

{% codeblock %}
$bind = array(':ID' => 'ABCDE12345');
$sql = "SELECT * FROM {$this->db->dbprefix('TABLE')} WHERE ID = :ID";
$this->db->query($sql, $bind);
{% endcodeblock %}

但今天改用`php`的方法來`bind`，使用`oci_bind_by_name`這方法，但是原本參考[Document](http://us2.php.net/manual/en/function.oci-bind-by-name.php)很直覺就是帶參數進去，並沒想太多!

但是發生了500 Error找了很久仔細看發現，其實是第三個參數的問題，在文件中第三個參數是使用Reference的方式，所以並不能使用字串，必須要在外面定義一個變數帶入。

{% codeblock %}
// 錯誤
oci_bind_by_name($stid, ":ID", "ABCDE12345");

// 正確
$id = "ABCDE12345";
oci_bind_by_name($stid, ":ID", $id);
{% endcodeblock %}

## 補充 2014/05/18

這也是學長發現的問題，在這邊記錄一下！

就是在`foreach`中使用並不能直接使用`$value`...看下面的code好了

{% codeblock %}
foreach($array as $key => $value) {
    // 這邊的$value都只會抓到最後一個的值
    // oci_bind_by_name($stid, ":ID", $value); // 會有問題

    // 如果要正確取得每次迴圈的值應該修改成如下
    oci_bind_by_name($stid, ":ID", $array[$key]); 
}
{% endcodeblock %}
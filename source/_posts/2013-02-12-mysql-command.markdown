---
layout: post
title: "mysql command"
date: 2013-02-12 15:48
comments: true
tags: 
- mysql
---

## 登入
{% codeblock %}

	>mysql -u root -h localhost -p

{%endcodeblock %}

## 顯示資料庫
{% codeblock %}

    >show databases;

{% endcodeblock %}


## 選擇使用的資料庫
{% codeblock %}

    >use database;

{% endcodeblock %}

## 顯示目前選擇的資料庫
{% codeblock %}

    >select database();
    
{% endcodeblock %}

## 顯示目前連線狀態
{% codeblock %}

    >status

{% endcodeblock %}

## 顯示該資料庫的表單(Table)
{% codeblock %}

    >show tables;

{% endcodeblock %}

## 顯示表單(Table)結構
{% codeblock %}

	>describe 資料表明稱;
	
{% endcodeblock %}

## 顯示資料表所有內容
{% codeblock %}

	>select * from 資料表明稱;

{% endcodeblock %}

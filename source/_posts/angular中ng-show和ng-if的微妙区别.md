---
title: angular中ng-show和ng-if的微妙区别
date: 2017-04-08 18:09:12
tags: angular
---

场景：最近在使用插件时，发现如果是使用ng-if时，jquery很有可能获取不到这个元素。但是ng-show是可以获取到的。    

分析：可能ng-if不会在页面进入的时候渲染。只有在条件符合的时候才会去触发；但是ng-show会在页面渲染的时候就会渲染，所以在使用插件的时候，ng-show可以直接获取到这个dom
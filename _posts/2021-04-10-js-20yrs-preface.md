---
layout:       post
title:        "Angular音频显示"
author:       "zhangxx"
header-style: text
catalog:      true
tags:
    - 前端
    - 后端
    - Angular
    - flask
---

> 在开发中遇到了在前端将后端传来的音频展示的需求。


这个问题需要从两方面出发，一是后端如何将音频文件传出，二是前端调了接口后怎么处理。
后端： cur_path = audio.transfer(path)
      file = open(cur_path, 'rb')
      return send_file(file, mimetype='audio/wav')
在这个过程中有一点需要注意，因为需要前后端都操作，可以利用postman先确保后端正确传输，再调整前端。
前端： fetch(url,{
      method: 'POST',
      body: JSON.stringify(data),
      headers: new Headers({
        'Content-Type': 'application/json'
      })
    })
      .then(response => response.blob())
      .then(blob => {
        const url = URL.createObjectURL(blob);
        console.log(url)
        const audio_element = new Audio(url);
        const  div = document.getElementById('div1');
        audio_element.controls=true;
        div!.appendChild(audio_element)
        audio_element.play();
      });

还有一种可行方案是后端用send_from_directory来传输文件，这样可能需要在前端对'Content-Type'进行修改。
